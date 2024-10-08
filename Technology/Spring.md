## Spring Framework

- Core 核心模块
IoC Container, Events, Resources, i18n, Validation, Data Binding, Type Conversion, SpEL, AOP, AOT.

- Testing 测试模块
Mock Objects, TestContext Framework, Spring MVC Test, WebTestClient.

- Data Access 数据访问模块
Transactions, DAO Support, JDBC, R2DBC, O/R Mapping, XML Marshalling.

- Web Servlet 
Spring MVC, WebSocket, SockJS, STOMP Messaging.

- Web Reactive	
Spring WebFlux, WebClient, WebSocket, RSocket.

- Integration 集成模块
REST Clients, JMS, JCA, JMX, Email, Tasks, Scheduling, Caching, Observability, JVM Checkpoint Restore.

- Languages 语言模块
Kotlin, Groovy, Dynamic Languages.

- Appendix	
Spring properties.

1. 动态代理的实现方式：
   - JDK动态代理，使用jdk中的`Proxy`, `Method`, `InvocationHandler` 创建代理(jdk动态代理要求目标类必须实现接口)
   - CGLIB动态代理：第三方的工具库，创建代理对象，原理是继承。
      > 通过继承目标类，创建子类。
      > 子类就是代理对象
      > 要求目标类不能是final的，方法也不能是final的
2. 动态代理的作用：
   - 在目标类源代码不改变的情况下，增加功能
   - 减少代码的重复
   - 专注业务逻辑代码
   - 解耦合，将业务功能和日志，事务、非业务功能分离
3. AOP(Aspect Orient Programming)：面向切面编程
   > Aspect：切面，表示给业务方法增加的功能，一般日志输出、事务、权限检查等功能用AOP实现
   > PointCut：切入点：一个或多个JoinPoint的集合，表示切面功能执行的位置
   > Advice：通知，也叫增强，表示切面的执行时间，在方法前或方法后等等
4. 理解AOP
   - 需要在分析项目功能时，找出切面
   - 合理安排切面的执行时间（在目标方法前，还是目标方法后）
   - 合理安全的切面执行位置，在哪个类，哪个方法增加增强功能
5. AOP的实现
   AOP是一个规范，是动态的一个规范化，一个标准
   AOP的技术实现框架
   - Spring（内部实现，主要用于事务处理，很少使用，较为笨重）
   - AspectJ（开源AOP框架，Spring框架继承了AspectJ框架，通过Spring就能使用AspectJ的功能）
   > 使用xml配置文件：全局
   > 使用注解(优先，AspectJ 5个注解)
6. 学习AspectJ框架的使用
   1. 切面的执行时间，规范叫做Advice（通知，增强）
   2. 在AspectJ框架中使用注解表示，或者xml配置文件中的标签
   > @Before (前置通知，在目标方法之前先执行切面的功能)(表示切面位置的切入点表达式：execution)
   > @AfterReturning
   > @Around
   > @AfterThrowing
   > @After
7. AOP的作用
   1. 在目标类不被修改源代码的情况下，增加功能
   2. 减少重复的代码
   3. 专注于业务功能的实现
   4. 解耦合：业务功能和体制、事务这些非业务功能的耦合

***什么时候考虑使用AOP技术***
- 当要给系统中存在的类修改功能，但原有类的功能不完善，无源代码，使用AOP增加功能
- 给项目中的类，都增加相同的功能，使用AOP
