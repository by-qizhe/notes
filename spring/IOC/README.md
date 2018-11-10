# IOC与DIP
IOC（Inversion Of Control）译名控制反转，基于依赖倒置（Dependency Inversion Principle）原则，两者都是思想而不是技术

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

"控制的什么被反转了？获得依赖对象的方式反转了"这句话在2004年初由Martin Fowler的一篇论文提出，能够很好的表达IOC是什么

> IOC控制反转本身不是技术而是思想，并且与DIP依赖倒置表达的都是同个意思
