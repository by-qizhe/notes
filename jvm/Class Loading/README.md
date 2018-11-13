# 类加载机制
在jvm中，有一个模块叫做类加载模块，负责加载类信息到内存中
类加载模块的加载时机是在运行中，当运行过程需要使用某个类时并且还未被加载到内存中，jvm就会使用该模块将该类加载到内存

## 类加载器
类加载模块的加载工具，由如下几个组成：

- BootStrap ClassLoader：主加载器。C++编写。负责加载jre/lib/下的核心类；
- Extend ClassLoader：二级加载器。Java编写。负责加载jre/lib/ext/下的扩展类；
- Application ClassLoader：三级加载器。Java编写。负责加载CLASS_PATH环境变量指定路径下的类；
- 自定义ClassLoader：自定义加载器。由开发者自定义加载；
