# Spring Boot是什么
Spring Boot是Pivotal团队开发的一个轻量级框架，通过"约定大于配置"习惯，提高项目开发效率和减少配置操作

# Spring Boot特性

- Create stand-alone Spring applications
- 可创建独自运行的Spring项目，以JAR包单独运行
- Embed Tomcat, Jetty or Undertow directly (no need to deploy WAR files)
- 项目直接嵌入Tomcat、Jetty或Undertow等容器（无需配置WAR文件）
- Provide opinionated 'starter' dependencies to simplify your build configuration
- 尽可能的提供配置默认值，以此来简化配置操作
- Automatically configure Spring and 3rd party libraries whenever possible
- 尽量的为Spring和第三方库进行自动配置
- Provide production-ready features such as metrics, health checks and externalized configuration
- 为项目提供度量、健康检查和外部化配置特性
- Absolutely no code generation and no requirement for XML configuration
- 不会生成代码且无需XML配置

# EnableAutoConfiguration注解
EnableAutoConfiguration是Spring Boot的核心注解，不难看出，该注解的功能是自动配置。通过注解在启动类上，Spring Boot就会对当前项目支持的依赖项进行自动配置，该注解节省了大量配置操作

## 自动配置流程
首先会去读取CLASS_PATH/META-INF/spring.factories这个文件，获取后缀为EnableAutoConfiguration参数的属性值，这里的属性值包含着多个全限定名，中间用了逗号隔开，Spring Boot会将这些属性值都先放到List中，然后使用反射（reflect）去获取每个类的信息，并根据这些类的类注解@ConditionXXX，来根据其条件选择是否放入IOC容器中。这里的ConditionXXX是决定Spring Boot是否要对这个类进行自动配置
