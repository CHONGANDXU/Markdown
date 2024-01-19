# 第一章 Spring Boot

## 1.1 注解的使用

@SpringBootApplication 【复合注解】  
由：

- @SpringBootConfiguration
- @EnableAutoConfiguration
- @ComponentScan

1. @SpringBootConfiguration
   ```java
   // 说明：使用了 @SpringBootConfiguration 注解标注的类，可以作为配置文件使用（使用Bean声明对象，注入容器）
   @Configuration
   public @interface SpringBootConfiguration {
       @AliasFor(
               annotation = Configuration.class
       )
       boolean proxyBeanMethods() default true;
   }
   ```
2. @EnableAutoConfiguration
    - 启动自动配置，把java对象配置后，注入到Spring容器中。（例如将mybatis对象创建好并注入容器）
3. @ComponentScan  
    - 扫描器，找到注解，根据注解的功能完成创建对象、给属性赋值等操作  
    - 默认扫描包的位置：@ComponentScan所在的类的包以及子包

## 1.2 核心配置文件(test-02 )

配置文件名称：application  
文件格式：properties(key=value) 或者 yml(key:value)   
例一：application.properties：设置端口和上下文路径

```properties
#设置端口号
server.port=8088
#设置访问应用的上下文路径
server.servlet.context-path=/myBoot
```

例二：application.yml

```yaml
server:
  port: 8085
  servlet:
    context-path: /myBoot2
```

## 1.3 多环境配置

开发环境，测试环境，上线环境  
每个环境有不同的配置文件，例如 端口，上下文件，数据库URL，用户名，密码

使用多环境配置文件，可以方便地切换不同的配置  
命名规则：application-环境名称.properties(yml)

```properties
#激活使用按需的配置文件
spring.profiles.active=dev
```

## 1.4 使用容器 (test-03.service)

想通过代码，从容器中获取对象  
通过SpringApplication.run(Test03Application.class, args);

```java
public interface ConfigurableApplicationContext extends ApplicationContext, Lifecycle, Closeable {
    //...
}
```  

ConfigurableApplicationContext :接口，是ApplicationContext的子接口

## 1.5 CommandLineRunner 和 ApplicationRunner 接口  (test-04)

这两个接口都有一个run方法，执行时间在容器对象创建好之后，自动执行run()方法  
可以完成自定义的在容器对象创建好后的一些操作

```java

@FunctionalInterface
public interface CommandLineRunner {
    void run(String... args) throws Exception;
}

@FunctionalInterface
public interface ApplicationRunner {
    void run(ApplicationArguments args) throws Exception;
}
```  

# 第二章 Web组件  (test-05)

三个方面：拦截器、Servlet、Filter

## 2.1 拦截器

拦截器是SpringMVC中的一种对象，能拦截对Controller的请求  
拦截器框架中有系统的拦截器，还可以自定义拦截器，实现对请求的预处理。

实现自定义的拦截器

- SpringMVC实现

>1. 创建类实现SpringMVC框架的HandlerInterceptor接口
```java
public interface HandlerInterceptor {
   default boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
       return true;
   }

    default void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, @Nullable ModelAndView modelAndView) throws Exception {
    }

    default void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, @Nullable Exception ex) throws Exception {
    }
}
```
> 2. 需要在SpringMVC的配置文件中，声明拦截器
```xml

<!-- 在Spring MVC的配置文件中注册拦截器 -->
<mvc:interceptors>
    <!-- 配置一个全局拦截器，拦截所有请求 -->
    <bean class="com.example.web.interceptor.MyInterceptor" />
    
    <!-- 配置一个指定路径的拦截器，只拦截/login开头的请求 -->
    <mvc:interceptor>
        <mvc:mapping path="/login/**" />
        <bean class="com.example.web.interceptor.LoginInterceptor" />
    </mvc:interceptor>
</mvc:interceptors>
```

- SpringBoot中注册拦截器：

```java

@Configuration
public class MyAppConfig implements WebMvcConfigurer {

    //添加拦截器对象，注入到容器中
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        //创建拦截器对象
        HandlerInterceptor handlerInterceptor = new LoginInterceptor();
        //指定拦截的请求URL地址
        String[] path = {"/user/**"};
        //指定不拦截的地址
        String[] excludePath = {"/user/login"};
        registry.addInterceptor(handlerInterceptor).addPathPatterns(path).excludePathPatterns(excludePath);
    }
}
```

## 2.2 Servlet

在 SpringBoot 框架中使用 Servlet 框架  
使用步骤：

1. 创建 Servlet 类，继承 HttpServlet

```java
//创建servlet类
public class MyServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doPost(req, resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //使用 HttpServletResponse 输出数据，应答结果
        resp.setContentType("text/html;charset=utf-8");
        PrintWriter out = resp.getWriter();
        out.println("==执行的是Servlet==");
        out.flush();
        out.close();
    }
}
```

2. 注册 Servlet ，让框架找到 Servlet (两种方式创建 ServletRegistrationBean 类)

```java

@Configuration
public class MyAppConfig {
    @Bean
    public ServletRegistrationBean servletRegistrationBean() {
        /*public ServletRegistrationBean(T servlet, String... urlMappings)
         * 第一个参数是 Servlet 对象，第二个是 URL 地址
         * */

        //ServletRegistrationBean bean = new ServletRegistrationBean(new MyServlet(),"/myServlet");

        ServletRegistrationBean bean = new ServletRegistrationBean();
        bean.addUrlMappings("/login", "/test");
        bean.setServlet(new MyServlet());
        return bean;
    }
}
```

## 2.3 Filter

在 SpringBoot 框架中使用 Filter 框架  
使用步骤：

1. 创建自定义过滤器类，继承 Filter

```java
//自定义过滤器
public class MyFilter implements Filter {
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        System.out.println("执行了自定义过滤器的doFilter");
        filterChain.doFilter(servletRequest, servletResponse);
    }
}
```

2. 注册 Filter

```java

@Configuration
public class MyAppConfig {
    @Bean
    public FilterRegistrationBean filterRegistrationBean() {
        FilterRegistrationBean bean = new FilterRegistrationBean();
        bean.setFilter(new MyFilter());
        bean.addUrlPatterns("/user/*");
        return bean;
    }
}
```

## 2.4 字符集过滤器 (CharacterEncodingFilter)

作用：解决post请求中的乱码问题    
在SpringMVC框架中，在web.xml注册过滤器，配置属性  
使用步骤：

- 第一种方式

1. 配置自定义字符集过滤器

```java

@Configuration
public class MyAppConfig {
    @Bean
    public FilterRegistrationBean filterRegistrationBean1() {
        FilterRegistrationBean bean = new FilterRegistrationBean();
        //使用框架中的过滤器类
        CharacterEncodingFilter filter = new CharacterEncodingFilter();
        //指定使用的编码方式
        filter.setEncoding("utf-8");
        //指定request，response 都使用 encoding 的值
        filter.setForceEncoding(true);

        bean.setFilter(filter);
        bean.addUrlPatterns("/*");
        return bean;
    }
}
```

2. 修改application.properties文件，让自定义的过滤器起作用

```properties
#SpringBoot中默认配置了CharacterEncodingFilter,编码默认为ISO-8859-1
#设置enabled=false,作用是关闭系统中配置好的过滤器,使用自定义的 CharacterEncodingFilter
server.servlet.encoding.enabled=false
```

- 第二种方式

> 直接修改application.properties文件

```properties
server.servlet.context-path=/myBoot
server.servlet.encoding.enabled=true
#指定使用的编码方式
server.servlet.encoding.charset=utf-8
#强制request response 都使用 charset 属性的值
server.servlet.encoding.force=true
```

# 第三章 ORM 操作 MySQL

使用mybatis框架操作数据，在SpringBoot框架中集成Mybatis
使用步骤：

1. mybatis起步依赖，完成mybatis对象自动配置，对象放在容器中
2. pom.xml指定将src/main/java 目录中 xml 文件包含在 classpath 中
3. 创建实体类 Student
4. 创建Dao接口 StudentDao，创建一个查询学生的方法
5. 创建Dao接口对应的Mapper文件 xml文件 SQL语句
6. 创建Service层对象，StudentService接口和它的实现类，调用dao对象的方法，完成对数据库的操作
7. 创建Controller对象，访问Service
8. 写application.properties文件——配置数据库的连接信息

## 第一种方式：@Mapper

@Mapper:放在dao接口的上面，每个接口都需要@Mapper注解【缺点】

```java
/*
 * 告知Mybatis，此为dao接口，创建此接口的代理对象
 * */
@Mapper
public interface StudentDao {
    Student selectStudentBySno(@Param("stuNo") Integer sno);
}
```

## 第二种方式：@MapperScan

@MapperScan：放在 **主启动类** 上，标注扫描的接口包路径 basePackages=""

```java

@SpringBootApplication
@MapperScan(basePackages = "com.demo.dao")
//@MapperScan(basePackages = {"com.demo.dao","其他路径"})
public class Test06OrmApplication {

    public static void main(String[] args) {
        SpringApplication.run(Test06OrmApplication.class, args);
    }

}
```

## 第三种方式：Mapper 文件和 Dao 接口 分开管理

把Mapper文件放在resources目录下

1. 在resources目录中创建子目录（自定义），例如mapper
2. 把mapper文件放入自定义的子目录下
3. 在application.properties文件中，指定mapper文件的目录

```properties
#指定 Mapper 文件的位置
mybatis.mapper-locations=classpath:mapper/*.xml
#指定mybatis的日志
mybatis.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl
```

4. 在pom.xml中 【指定】 把 resources 目录中的文件，编译到目标目录中

```xml

<resources>
    <!--以下为dao与mapper文件在同一目录下的配置-->
    <!--            <resource>
                    <directory>src/main/java</directory>
                    <includes>
                        <include>**/*.xml</include>
                    </includes>
                </resource>-->

    <!--以下为dao与mapper文件分开的配置-->
    <resource>
        <directory>src/main/resources</directory>
        <includes>
            <include>**/*.*</include>
        </includes>
    </resource>
</resources>
```

## 第四种方式： 事务

### Spring框架中的事务：

> 1. 管理事务的对象：事务管理器（接口，接口的实现类）
>> 例如：使用JDBC或Mybatis访问数据库，使用的事务管理器：DataSourceTransactionManager
> 2. 声明式事务：在【xml配置文件】或者【使用注解】 说明事务控制的内容
> - 控制事务：隔离级别，传播行为，超时时间
> 3. 事务处理方式
>     - Spring 框架中使用@Transactional
>     - AspectJ 框架可以在xml配置文件中，声明事务控制的内容

### SpringBoot中使用事务，以上两种方式都可以

1. 在业务方法上加入 @Transactional ，加入注解后，方法既有事务功能
2. 明确地在 【主启动类】 上，加入@EnableTransactionManager

```java
/*
 @Transactional:表示此方法有事务支持
*       默认：使用数据库的隔离级别，required传播行为，-1为超时时间
*       抛出运行时异常，回滚事务
*/
 
@Transactional
@Override
public int addStudent(Student student){
    System.out.println("业务方法addStudent");
    int rows=studentMapper.insert(student);
    System.out.println("执行SQL语句");
    //抛出一个运行时异常，目的是回滚事务
    int m=10/0;
    return rows;
}
```

# 第四章 接口架构风格--REST ful

接口：应用程序接口（英语：Application Programming
Interface，简称：API），又称为应用编程接口，就是软件系统不同组成部分衔接的约定。API之主要目的是提供应用程序与开发人员以访问一组例程的能力，而又无需访问源码，或理解内部工作机制的细节。提供API所定义的功能的软件称作此API的实现。API是一种接口，故而是一种抽象。  
接口：可以指访问servlet，controller的url，调用其他程序的函数  
架构风格：API的组织方式（样子）

- 传统的：http://localhost:9090/myTrans/addStudent?name=lisi&age=26
    - 地址提供了访问的资源名称addStudent，在其后使用了get方式传递参数
    - REST ful
        - REST(Representational State Transfer ， 表现层状态转移)
        - REST：是一种架构风格和设计的理念，不是标准
        - 优点：更简洁，更有层次
        - REST要素：用REST表示资源和对资源的操作。在互联网中，表示一个资源或者一个操作。
            - 资源使用URL表示，图片、视频、文本、网页等等都是资源
            - 资源用名词表示
                - 查询：看，通过URL找到资源
                - 创建：添加资源
                - 更新：更新资源，编辑
                - 删除：去除
            - 资源使用URL表示，通过名词表示资源
                - 在URL中，使用名词表示资源，以及访问资源的信息，使用”/“分隔资源的信息
            - 使用HTTP中的请求方式，表示对资源的操作(CURD)
                - GET:select
                    - 处理单个资源：http://localhost:8080/myboot/student/1001
                    - 处理多个资源：http://localhost:8080/myboot/student/1001/1002
                - POST:insert(http://localhost:8080/myboot/student)
              ```html
              <from action="http://localhost:8080/myboot/student" method="post">  
                  姓名：<input type="text" name="name"/>
                  年龄：<input type="text" age="age"/>
              </from>
              ```
                - PUT:update
              ```html
              <from action="http://localhost:8080/myboot/student/1" method="post">  
                姓名：<input type="text" name="name"/>
                年龄：<input type="text" age="age"/>
                     <input type="hidden" name="_method" value="PUT"/>
              </from>
              ```
                - DELETE:delete

## 4.1 注解使用

@PathVariable:从URL中获取数据

@GetMapping:支持的GET请求方式，=@RequestMapping(method=RequestMethod.GET)

@PostMapping:支持的POST请求方式，=@RequestMapping(method=RequestMethod.POST)

@PutMapping:支持的PUT请求方式，=@RequestMapping(method=RequestMethod.PUT)

@DeleteMapping:支持的DELETE请求方式，=@RequestMapping(method=RequestMethod.DELETE)

@RestController:复合注解，相当于类中所有的方法都添加了@ResponseBody

# 第八章 Thymeleaf模板

Thymeleaf：是使用java开发的模板技术，在服务器端运行，将处理后的数据发送给浏览器

- 模板是在视图层工作的，用于显示数据
- 基于HTML语言
- 语法应用在HTML标签种
- SpringBoot框架集成Thymeleaf

## 8.1 表达式

1. 标准变量表达式

> 语法：${key}  
> 作用：获取key对于的文本数据，key是request作用域中的key，使用request.setAttribute(),model.addAttribute()  
> 使用：在页面的HTML标签中，使用 th:text="${key}"

```html

<div style="margin-left: 400px">
    <h3>标准变量表达式 ${key}</h3>
    <p th:text="${site}"></p>
    <br>
    <p>获取SysUser对象，属性值</p>
    <p th:text="${myUser.id}">ID</p>
    <p th:text="${myUser.name}">姓名</p>
    <p th:text="${myUser.sex}">性别：Male 男</p>
    <p th:text="${myUser.age}">年龄</p>
    <br>
    <p th:text="${myUser.getName()}">获取姓名使用GET方法</p>
</div>
```

2. 选择变量表达式(星号变量表达式)

> 语法：*{key}
> 作用：获取这个key对应的数据，*{key} 需要和 th:object 这个属性一起使用
> 目的：简单获取对象的属性值

```html
    <p>使用 *{} 获取 SysUser 的属性值</p>
<div th:object="${myUser}">
    <p th:text="*{id}">ID</p>
    <p th:text="*{name}">姓名</p>
    <p th:text="*{sex}">性别：Male 男</p>
    <p th:text="*{age}">年龄</p>
</div>
```

3. 链接表达式

> 语法：@{url}  
> 作用：表示链接

```text
  <script src="...">
  <link href="...">
  <a href="...">
  <form action="...">
  <img src="...">
```

```html
<h3>链接绝对数据</h3>
<a th:href="@{https://www.baidu.com}">链接到百度</a>

<h3>链接相对地址，无参</h3>
<a th:href="@{/tpl/queryAccount}">相对地址，没有参数</a>

<h3>链接相对地址，使用字符串链接传递参数</h3>
<a th:href="@{'/tpl/queryAccount?id='+${userId}}">相对地址，获取model中数据</a>

<h3>推荐使用的传参方式</h3>
<a th:href="@{/tpl/queryAccount(id=${userId})}">传参数</a>

<h3>传递多个参数</h3>
<a th:href="@{/tpl/queryUser(name='lisi',age=18)}">传多个参数</a>
```

## 8.2 Thymeleaf属性

属性是放在html元素中的，属性的作用不变，属性的值由模板引擎处理。  
在属性中可以使用变量表达式

```html

<form action="/loginServlet" method="post"></form>
<form th:action="/loginServlet" th:method="${methodAttr}"></form>
```

