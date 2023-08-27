# Maven学习

- 什么是maven

  Maven是Apache的一款开源的项目管理工具。以后无论是普通 `JavaSE` 项目还是 `JavaEE` 项目，我们都创建的是Maven项目。Maven使用项目对象模型(POM-Project Object Model，项目对象模型)的概念，可以通过一小段描述信息来管理项目的构建，报告和文档的软件项目管理工具。在Maven中每个项目都相当于是一个对象，对象（项目）和对象（项目）之间是有关系的。关系包含了：依赖、继承、聚合，实现Maven项目可以更加方便的实现导jar包、拆分项目等效果。



- Maven的必要性：

  由于 Java 的生态非常丰富，无论你想实现什么功能，都能找到对应的工具类，这些工具类都是以 jar 包的形式出现的，例如 Spring，SpringMVC、MyBatis、JDBC，等等，都是以 jar 包的形式出现的，jar 包之间会有关联，在使用一个依赖之前，还需要确定这个依赖所依赖的其他依赖，所以，当项目比较大的时候，依赖管理会变得非常麻烦臃肿，这是 Maven 解决的第一个问题。

  Maven 还可以处理多模块项目。简单的项目，单模块分包处理即可，如果项目比较复杂，要做成多模块项目，例如一个电商项目有订单模块、会员模块、商品模块、支付模块...，一般来说，多模块项目，每一个模块无法独立运行，要多个模块合在一起，项目才可以运行，这个时候，借助 Maven 工具，可以实现项目的一键打包。



- Maven的两大核心
  - 依赖管理：对`jar`包的统一管理
  - 项目构建：对项目进行编译、测试、打包、部署、上传等等



- Maven坐标：
  - `groupId` 公司或组织域名倒序
  - `artifactId` 模块名，也是实际项目的名称
  - `version` 当前项目的版本
  - `scope` 依赖范围

```xml
<groupId>com.mysql</groupId>
<artifactId>mysql-connector-j</artifactId>
<version>8.0.33</version>
<scope></scope>
```



- Maven依赖范围`<scope></scope>`

  - compile

    > 默认范围，如果无指定，则使用该依赖范围，表示该依赖在<font color="orange">编译和运行时都生效</font>

  - provided

    > 已提供依赖范围。使用<font color="orange">此依赖范围</font>的Maven依赖。典型的例子是servlet-api，编译和测试项目的时候需要该依赖，但在运行项目的时候，由于容器已经提供，就不需要Maven重复地引入一遍

  - runtime

    > runtime范围表明编译时不需要生效，<font color="orange">只在运行时生效</font>。典型的例子是 `JDBC` 驱动实现，项目主代码的编译只需要 `JDK` 提供的  `JDBC` 接口，只有在执行测试或者运行项目的时候才需要实现上述接口的具体 `JDBC` 驱动。 

  - system

    > 系统范围与provided类似，不过你必须显式指定一个<font color="orange">本地系统路径的`JAR`</font>，此类依赖应该一直有效，Maven也不会去仓库中寻找它。但是，使用system范围依赖时必须通过<font color="orange">`systemPath`</font>元素显式地指定依赖文件的路径。

  - test

    > test范围表明使用此依赖范围的依赖，只在编译测试代码和运行测试的时候需要，应用的正常运行不需要此类依赖。典型的例子就是 junit，它只有在编译测试代码及运行测试的时候才需要。junit的jar包就在测试阶段用就行了，你导出项目的时候没有必要把junit的东西到处去了就，所在在junit坐标下加入`<scope>test</scope>`
  
  - import
  
    > import范围只适用于pom文件中的<dependencyManagement>部分.  
    >
    > 表明指定的pom必须使用<dependencyManagement>部分的依赖.
    >
    > > 注意:import只能用在dependencyManagement的scope里.
  
    
  
- Maven指令

  - clean

    >清除由项目编译创建的target

  - validate

    > 验证项目是否正确并且所有必要的信息均可用

  - compile

    >编译项目的源代码

  - test

    > 使用合适的单元测试框架来测试编译的源代码。 这些测试不应要求将代码打包或部署

  - package

    > 完成了项目编译、单元测试、打包功能，但没有把打好的可执行jar包（war包或其它形式的包）布署到本地maven仓库和远程maven私服仓库

  - verify

    > 对集成测试的结果进行任何检查，以确保符合质量标准

  - install

    > 完成了项目编译、单元测试、打包功能，同时把打好的可执行jar包（war包或其它形式的包）布署到本地maven仓库，但没有布署到远程maven私服仓库

  - site

    > 生成项目相关信息的网站

  - deploy

    > 完成了项目编译、单元测试、打包功能，同时把打好的可执行jar包（war包或其它形式的包）布署到本地maven仓库和远程maven私服仓库









