# AOP
AOP全称Aspect Oriented Programming，即面向切面编程，其是一种技术，使用面向切面技术，能够减少大量的重复代码，保证在不修改原有代码的前提下增加新的功能
而目前的AOP技术分为静态切面和动态切面，两者的实现方式有差异，但最终实现的效果是一致的，Spring的AOP使用的是动态切面技术

## AOP能解决什么
- 提高代码利用率、减少代码冗余度；
- 无需改动原有代码实现功能添加；
- 允许调用方法全方位拦截（调用前、调用后、触发异常等）；

## 静态与动态
切面有分为静态和动态，静态指在编译期进行植入，而动态则在运行期植入

### 静态切面
早期的切面中，只有静态切面技术，是由Java语言编写的AspectJ技术来实现。作为静态切面技术的AspectJ，采用了独自的标签和编译器来实现此项技术，AspectJ的独自编译器可以编译自身独有的标签代码，还可以编译原有的Java代码，但由于需要花额外成本学习这些多出的标签，和规定必须由AspectJ编译器编译这些标签，使用起来并不是很方便，这也是后面的Spring AOP采用动态切面的原因之一

### 动态切面
不同于静态切面技术，其动态是在运行时进行切面的，Spring AOP的运行时切面，能够减少静态切面的繁琐（独自编译器编译、额外的标签）操作

### Spring AOP
Spring AOP使用了jdk反射代理和第三方cglib字节码框架来实现，使用了两种是因为jdk反射代理有局限性（基于接口），只能代理接口的方法
对于接口没有定义的方法就无法进行代理，而后者cglib是基于继承的，能够很大程度弥补jdk反射代理，但对于私有化方法、final方法或final类
这种情况，都还是无法正常代理。在目标方法实现了接口时，Spring AOP会优先使用jdk动态代理

#### jdk反射与cglib区别
- jdk反射代理由jdk提供，cglib由第三方开发；
- jdk反射代理生成的类少并且简单易懂，cglib生成的类多并且较为复杂；
- jdk反射代理基于接口，cglib基于继承；
- jdk反射调用目标方法，cglib直接调用目标方法（cglib采用继承所以是super.method(xx)调用，识别方法采用索引号形式）；
- jdk反射无法代理final接口，cglib无法代理final类、final方法、private方法、static方法；