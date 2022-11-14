# Vue
1. Vue组件通信，父传子-用什么传递数据，关键字段是什么

   父组件通过自定义属性名+数据（要传递）=》v-bind:value="数据"

   关键字段：

   ```
   // child是子组件，name是传递的属性对象，“零远航”是值
   <child :name="零远航"></child>
   ```

   

2. Vue里绑定事件的方法有哪些？

   v-on: 事件绑定机制

   形式如：v-on:click  缩写为 @click;

3. MVVM框架由那几部分构成

   Model-View-ViewModel 模式
   - Model: 域模型，用于持久化
   - View: 作为视图模板存在
   - ViewModel: 作为视图的模型，为视图服务

4. Vue.js 的核心是什么？

   数据双向绑定

5. Vue中路由实现什么功能，由哪个模块实现？

   Vue中路由主要做的事情就是监听事件并分发执行事件处理函数，简单来说就是一些实现页面跳转功能，页面导航功能，由vue-router模块实现

6. css和html的样式有什么不同

   1、HTML是网页的结构，CSS是网页的样式。

   2、HTML由围绕内容的标签组成，而CSS由一个声明块继承的选择器组成。

   3、HTML是一门超文本标记语言，而CSS是一个层叠样式表，CSS依赖于HTML的结构

7. V-on 可以简写成什么？

   一个@

8. Vue中可以遍历的指令是什么？

   v-for

9. Vue里父子通信，父传子如何实现，具体步骤

   在父组件中，以标签形式使用子组件的时候，可以通过属性绑定，为子组件传递数据： 父组件中需要书写的:

   ```vue
   <my-son :pmsg1="parentMsg" :pinfo="parentInfo"></my-son>
   
   ....
   
   data(){
       return {
          parentMsg:'我是父组件' ,
          parentInfo:'我是父组件的信息'
       }
   }
   
   ```
在子组件中，如果向用父组件传递过来的数据，必须先定义 props 数组来接收：
   ```vue
   <script>
     export default {
       data(){
         return {}
       },
       methods: {},
       // property 属性
       // 注意：父组件传递到子组件中的数据，必须经过 props 的接收，才能使用；
       // 通过 props 接收的数据，直接可以在页面上使用；注意：不接受，不能使用外界传递过来的数据
       props: ['pmsg1', 'pinfo']
     }
   </script>
   
   ```

   

10. Vue里父子通信，子传父如何实现，具体步骤

    条件: 子组件数据已知,父组件数据未知,父组件想要使用子组件的方法,这时就需要子传父

    如果父向子传递方法，需要使用 事件绑定机制：

    ```vue
     <my-son @func="show"></my-son>
     ...
     methods:{
        //这个methods方法需要在父组件中定义这个方法:
        show(val){
        //这个val是一个形参,真正的参数,是在调用的时候传过来的参数
            console.log(val)
        }
     }
     
     注意:这里只是在父组件中定义了这个方法,真正的调用还是在子组件中调用这个方法,传参也是由子组件来进行传参
    
    ```

    子组件调用方法:

    ```vue
    methods: {
        // 点击子组件中的按钮,触发按钮的点击事件
        btnHandle(){
          // 在子组件中通过this.$emit()方法,触发父组件,为子组件绑定func事件
    
          this.$emit('func' +this.msg)
          //this.msg才是真正的实参,传的值也是this.msg
        }
      },
    
    ```

    - 子传父的具体步骤:
    - 子向父传值，要使用事件绑定机制@；
    - 父向子传递一个方法的引用
    - 子组件中，可以使用 this.$emit() 来调用父组件传递过来的方法
    - 在使用this.$emit()调用 父组件中方法的时候，可以从第二个位置开始传递参数；把子组件中的数据，通过实参，传递到父组件的方法作用域中；
    - 父组件就可以通过形参，接收子组件传递过来的数据；

11. Vue指令的功能（单击数据绑定的功能）

    实际上Vue指令的本质上就是自定义属性，分为内置指令和自定义指令

12. Vue的双向数据绑定是由哪个模块实现，原理是是什么？具体步骤是什么？

    由v-model指令进行双向数据绑定，v-model 时一个语法糖，它做了：绑定数据value、触发输入事件input、data 更新触发重新渲染

    针对不同的版本：

    在vue2.0中v-model语法糖简写的代码 `<Son :value="msg" @input="msg=$event" />`

    在vue3.0中v-model语法糖有所调整：`<Son :modelValue="msg" @update:modelValue="msg=$event" />`

    下面我用Vue3.X来进行演示

    下面是父组件

    ```vue
    <template>
      <div class="container">
        <!-- 如果你想获取原生事件事件对象 -->
        <!-- 如果绑定事函数 fn fn(e){ // e 就是事件对象 } -->
    	<!-- <h1 @click="fn"></h1>, 但是也可以用$event来表示 -->
        
        <!-- 如果绑定的是js表达式  此时提供一个默认的变量 $event -->
        <h1 @click="$event.target.style.color='red'">父组件 {{count}}</h1>
        <hr>
        <!-- 如果你想获取自定义事件  -->
        <!-- 如果绑定是函数 fn fn(data){ // data 触发自定义事件的传参 } -->
        <!-- 如果绑定的是js表达式  此时 $event代表触发自定义事件的传参 -->
        <!-- <Son :modelValue="count" @update:modelValue="count=$event" /> -->
        <Son v-model="count" />
      </div>
    </template>
    <script>
    import { ref } from 'vue'
    import Son from './Son.vue'
    export default {
      name: 'App',
      components: {
        Son
      },
      setup () {
        const count = ref(10)
        return { count }
      }
    }
    </script>
    ```

    下面是子组件：

    ```vue
    <template>
      <div class="container2">
        <h2>子组件 {{modelValue}} <button @click="fn">改变数据</button></h2>
      </div>
    </template>
    <script>
    export default {
      name: 'Son',
      props: {
        modelValue: {
          type: Number,
          default: 0
        }
      },
      setup (props, {emit}) {
        const fn = () => {
          // 改变数据
          emit('update:modelValue', 100)
        }
        return { fn }
      }
    }
    </script>
    ```

# Spring+SpringBoot+Mybatis
1. SpringBoot 历史，框架的作用，为啥要学习框架
> 2012 年 10 月，Mike Youngstrom 在 Spring jira 中创建了一个功能请求 ， 要求在 Spring 框架中支持无容器 Web 应用程序体系结构。他谈到了在主容器引导 spring 容器内配置 Web 容器服务。这是 jira 请求的摘录：我认为 Spring 的 Web 应用体系结构可以大大简化，如果它提供了从上到下利用 Spring 组件和配置模型的工具和参考体系结构。在简单的 main()方法引导的 Spring 容器内嵌入和统一这些常用Web 容器服务的配置。

>- 为什么要使用Spring Boot
> 1. 因为Spring SpringMVC 需要使用大量的配置文件(xml)，还需要配置各种对象，把使用的对象放入到Spring容器中才能使用对象，需要了解其他框架的配置规则
> 2. SpringBoot不需要配置文件，常用的框架和第三方库已经配置好，可以直接使用
> 3. 开发效率高，使用方便

2. SSM 包含哪些内容
Spring+SpringBoot+Mybatis

3. SSM 如何配置

4. 如何创建项目 SpringBoot 的项目-(IDEA)里要包含 SpringMVC
    - 一种方式，创建Maven项目，在 pom.xml 依赖文件中手动加入(spring-boot-starter/spring-boot-starter-web/spring-webmvc) 依赖
    - 另一种方式，使用IDEA中Spring Initializr(Spring项目初始化器)，选择适当的依赖(Spring Web)进行项目初始化
5. 如何根据一个数据表创建实体类
将数据库表名转换成实体类对象名，表中属性转换成类对象中的属性

6. 掌握 Mybatis 的xml配置文件
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <!--settings:控制mybatis全局行为-->
    <settings>
        <!--设置mybatis输出日志-->
        <setting name="logImpl" value="STDOUT_LOGGING"/>
    </settings>

    <!--环境配置：数据库的连接信息、
        dafault:必须和某人environment的id值一样。
        告诉mybatis使用哪个数据库的连接信息，也就是访问哪个数据库
    -->
    <environments default="mydev">
        <!--environment：一个数据库信息的配置，环境
            id:一个唯一值，自定义，表示环境名称
        -->
        <environment id="mydev">
            <!--
                transactionManager:mybatis的事务类型
                type:JDBC(表示使用jdbc中的Connection对象的commit，rollback)
            -->
            <transactionManager type="JDBC"/>
            <!--
                dataSource:表示数据源，连接数据库的
                type:表示数据源的类型，POOLED表示使用连接池
            -->
            <dataSource type="POOLED">
                <!--数据库的驱动类型名-->
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <!--连接数据库的url字符串-->
                <property name="url" value="jdbc:mysql://localhost:3306/****"/>
                <!--访问数据库的用户名-->
                <property name="username" value="****"/>
                <!--密码-->
                <property name="password" value="****"/>
            </dataSource>
        </environment>
    </environments>

    <!--sql mapper（sql映射文件）的位置-->
    <mappers>
        <!--一个mapper标签指定一个文件的位置
            从类路径开始的路径信息 target/clasess(类路径)
        -->
        <mapper resource="com/../*.xml"/>
    </mappers>
</configuration>

```
7. spring boot 的核心注解-@SpringBootApplication 包含哪些注解
@SpringBootApplication 【复合注解】  
由：
- @SpringBootConfiguration
- @EnableAutoConfiguration
- @ComponentScan


>1. @SpringBootConfiguration
>
```java
@Configuration
public @interface SpringBootConfiguration {
    @AliasFor(
            annotation = Configuration.class
    )
    boolean proxyBeanMethods() default true;
}
```
>说明：使用了 @SpringBootConfiguration 注解标注的类，可以作为配置文件使用（使用Bean声明对象，注入容器）

>2. @EnableAutoConfiguration  
>启动自动配置，把java对象配置后，注入到Spring容器中。（例如将mybatis对象创建好并注入容器）

>3. @ComponentScan  
>扫描器，找到注解，根据注解的功能完成创建对象、给属性赋值等操作  
>默认扫描包的位置：@ComponentScan所在的类的包以及子包