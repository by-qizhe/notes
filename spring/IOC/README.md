# IOC与DIP
IOC（Inversion Of Control）译名控制反转，基于依赖倒置（Dependency Inversion Principle）原则
而依赖注入（Dependency Injection）是IOC的实现方式

DIP官方定义：

    High level modules should not depend upon low level modules. Both should depend upon abstractions. 
    高层模块不应该依赖底层模块，而是应该依赖抽象。
    Abstractions should not depend upon details. Details should depend upon abstractions.
    抽象不应该依赖细节，而细节应该依赖抽象。

并不很难理解，为了避免不同模块间的直接调用形成的直接依赖，DIP将模块间的依赖进行改变，所有模块都通过抽象，来间接调用目标模块
非直接依赖目标模块能够很大程度减少耦合，当需要换成其它模块也无需做太多修改，这与面向接口编程极为相似

那IOC呢？上面已说到是基于依赖倒置原则，自然不会与DIP相差太大，刚好IOC本质也是将依赖的对象改变
而依赖注入（Dependency Injection）是IOC的常见实现方式，DI能够帮助开发者实例化对象，开发者不需要去敲繁琐的new操作
DI能防止对对象的直接依赖，这也是一种解耦方式
> 取自Martin Fowler对IOC的作用："控制的什么被反转了？获得依赖对象的方式反转了"

# Spring IOC
Spring采用了IOC思想，并且其已经成为Spring的两大核心功能之一（IOC与AOP）
其实现方式采用容器方式，容器内存储Bean的类信息、配置信息与实例缓存，并且外部也能够获取容器的信息
将类定义成Bean，Spring支持XML、Java注解或JavaConfig形式
获取容器的Bean实例，Spring也是支持XML、Java注解与JavaConfig形式

#### Bean作用域
Spring允许为Bean设置作用域（Bean即是类），支持如下作用域：
- Singleton：默认配置。单例。当Bean没有配置lazy-init懒加载属性，在容器初始化时会先实例化缓存在容器；
- Prototype：原型。每次获取Bean的实例Spring都会重新实例化一个新的；
- Session：会话。在相同客户端下，返回的始终是同个实例；
- Request：请求。同会话，同个请求下，Bean的实例始终相同；
- Global Session：全局会话；

### BeanFactory与ApplicationContext
早期的Spring实现IOC容器功能中，采用的是BeanFactory接口来定义基本功能
而后来为了在容器上添加高级的特性
而ApplicationContext是基于BeanFactory升级的，用来添加容器的高级特性
目前普遍都采用ApplicationContext而非BeanFactory

### 容器创建过程
Spring IOC的容器，分为初始化与使用两个阶段

#### 初始化阶段
Spring IOC的ApplicationContext接口有着很多实现类，常见的ClassPathXmlApplicationContext与FileSystemXmlApplicationContext
以XMl配置为例，在容器初始化时，都会传入配置文件的路径，而初始化阶段所做的活，正是加载配置文件中的Bean属性信息到容器
同时会对Bean提前实例化（前提条件成立），Beans中也支持replaced-method和lookup-method来对方法修改，由于会修改到Bean类的方法
所以Spring IOC借助了AOP技术来实现此项功能

#### 使用阶段
容器初始化完毕后，接下来就可以从容器中获取Bean实例，获取实例时，Spring IOC会根据其属性来做对应的操作并返回，若需存放到成员属性
则会以反射形式注入到目标字段

### 循环依赖与三级缓存
尽管Spring IOC能够帮助开发者提供Bean实例，但也存在了不同问题
循环依赖就是一个比较严重的例子，而Spring IOC提供的解决方案就是三级缓存
但要注意的是三级缓存也是无法彻底解决循环依赖的，有局限性
好，先来讲讲循环依赖

#### 什么是循环依赖
相信用过Spring IOC的Bean依赖注入就不陌生，IOC可以注入类的实例，也可以"顺便"帮其成员属性注入所需要的类实例
那么，当两个Bean的成员属性互相指向对方，就会出现循环依赖
因为IOC在实例化Bean需要分成几个步骤，上面有说到，还要为其成员属性注入
而在为成员属性依赖注入，就避免不了有可能出现循环依赖这种问题

#### 什么是三级缓存
那么，出现问题总要解决，而三级缓存就是被此设计而出
三级缓存看其名字就能知道大概，没错，是用了三个缓存来实现的
其底层机构是HashMap和ConcurrentHashMap，设计出三级，是因为配合Bean的实例多步骤
从大体上看，三级缓存是以下三个成员属性：
    
    /** Cache of singleton objects: bean name --> bean instance */
    private final Map<String, Object> singletonObjects = new ConcurrentHashMap<String, Object>(256);

    /** Cache of singleton factories: bean name --> ObjectFactory */
    private final Map<String, ObjectFactory<?>> singletonFactories = new HashMap<String, ObjectFactory<?>>(16);
    
    /** Cache of early singleton objects: bean name --> bean instance */
    private final Map<String, Object> earlySingletonObjects = new HashMap<String, Object>(16);

- singletonObjects：缓存bean的实例，单例的bean实例就缓存在这里；
- singletonFactories：单例Bean的工厂实例；
- earlySingletonObjects：提前曝光的bean实例，正是这个解决了循环依赖。获取Bean的实例，IOC首先会在singletonObjects中
                         查询是否有该Bean实例的缓存，有则返回，没有则对该Bean类进行实例化，这里是关键，在实例化Bean类
                         完成后，会将该实例放到earlySingletonObjects中，这里的操作直接解决了循环依赖问题，尽管这个类
                         并不完整（因为其成员属性可能需要依赖注入对应的实例），接下来在检查成员属性是否要依赖注入，需要
                         的话就去执行对应的依赖注入，如果出现循环依赖也没事，因为IOC也会在earlySingletonObjects中尝试
                         获取；
