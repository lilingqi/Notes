# 模板引擎（Thymeleaf）and web静态资源

## 1、什么是模板引擎

​    前端交给我们的页面，是html页面。如果是我们以前开发，我们需要把他们转成jsp页面，jsp好处就是当我们查出一些数据转发到JSP页面以后，我们可以用jsp轻松实现数据的显示，及交互等。

   jsp支持非常强大的功能，包括能写Java代码，但是呢，我们现在的这种情况，SpringBoot这个项目首先是以jar的方式，不是war，像第二，我们用的还是嵌入式的Tomcat，所以呢，==他现在默认是不支持jsp的==。

   ==那么默认不支持jsp界面我们应该怎么办：这个时候我们可以使用模板引擎Thymeleaf（springboot推荐）==

   模板引擎，我们其实大家听到很多，其实jsp就是一个模板引擎，还有用的比较多的freemarker，包括SpringBoot给我们推荐的Thymeleaf，模板引擎有非常多，但再多的模板引擎，他们的思想都是一样的，什么样一个思想呢我们来看一下这张图：

![image-20210716092325091](C:\Users\李令齐\AppData\Roaming\Typora\typora-user-images\image-20210716092325091.png)

​     模板引擎的作用就是我们来写一个页面模板，比如有些值呢，是动态的，我们写一些表达式。而这些值，从哪来呢，就是我们在后台封装一些数据。然后把这个模板和这个数据交给我们模板引擎，模板引擎按照我们这个数据帮你把这表达式解析、填充到我们指定的位置，然后把这个数据最终生成一个我们想要的内容给我们写出去，这就是我们这个模板引擎，不管是jsp还是其他模板引擎，都是这个思想。只不过呢，就是说不同模板引擎之间，他们可能这个语法有点不一样。



## 2、Thymeleaf的用法

### 2.1、thymeleaf怎么引用

   Thymeleaf 官网：https://www.thymeleaf.org/

   Thymeleaf 在Github 的主页：https://github.com/thymeleaf/thymeleaf

   Spring官方文档：找到我们对应的版本

   https://docs.spring.io/spring-boot/docs/2.2.5.RELEASE/reference/htmlsingle/#using-boot-starter 

 主要是引入下面这组依赖就可以用了

```java
 <dependency>
 			<groupId>org.thymeleaf</groupId>
 			<artifactId>thymeleaf-spring5</artifactId>
 </dependency>
 <dependency>
 			<groupId>org.thymeleaf</groupId>
 			<artifactId>thymeleaf-extras-java8time</artifactId>
 </dependency>
```



### 2.2、thymeleaf在springboot怎么用

  1.我们先来分析一下thymeleaf的源码

```java
@ConfigurationProperties(
    prefix = "spring.thymeleaf"
)
public class ThymeleafProperties {
    private static final Charset DEFAULT_ENCODING;
    public static final String DEFAULT_PREFIX = "classpath:/templates/";
    public static final String DEFAULT_SUFFIX = ".html";
    private boolean checkTemplate = true;
    private boolean checkTemplateLocation = true;
    private String prefix = "classpath:/templates/";
    private String suffix = ".html";
    private String mode = "HTML";
    private Charset encoding;
}
```

我们可以在其中看到默认的前缀和后缀！

我们只需要把我们的html页面放在类路径下的templates下，thymeleaf就可以帮我们自动渲染了。

使用thymeleaf什么都不需要配置，只需要将他放在指定的文件夹下即可！

==但是要想在html文件中得到渲染后的效果或者说要想在静态页面中动态的得到引用的样式和model中封装的对象的数据必须要有下面的这一步==

```java
在html界面中的html标签后面加入
xmlns:th="http://www.thymeleaf.org"
```

### 2.3、thymeleaf的语法

1. thymeleaf官方文档：https://www.thymeleaf.org/ 

2. 代码示例

   ```java
   @Controller
   public class IndexController {
       @RequestMapping("/myThymeleaf")
       public String myThymeleaf(Map<String, Object> result) {
           result.put("user", new UserEntity("mayikt", 22));
           return "myThymeleaf";
       }
   }
   
   ```

   

   ```java
   <!DOCTYPE html>
   <!--需要在HTML文件中加入以下语句： -->
   <html lang="en" xmlns:th="http://www.thymeleaf.org">
   <head>
       <meta charset="UTF-8">
       <title>Show User</title>
   </head>
   <body>
   <table>
       姓名:<span th:text="${user.userName}"></span>
       年龄:<span th:text="${user.age}"></span>
   </table>
   </body>
   </html>
   
   ```

   ```java
   //循环语句
   <ul th:each="user:${userList}">
       <li th:text="${user.userName}"></li>
       <li th:text="${user.age}"></li>
       <br>
   </ul>
   
   ```

   ```java
   //if语句
   <span th:if="${user.age>17}">已经成年啦</span>
   <span th:if="${user.age<17}">未成年</span>
   
   ```

   

3. 常用的一些关键字

   ![image-20210716094138464](C:\Users\李令齐\AppData\Roaming\Typora\typora-user-images\image-20210716094138464.png)

## 3、springboot中的静态资源处理

1. 前言

   写请求非常简单，那我们要引入我们前端资源，我们项目中有许多的静态资源，比如css，js等文件，这个SpringBoot怎么处理呢？

   如果我们是一个web应用，我们的main下会有一个webapp，我们以前都是将所有的页面导在这里面的，对吧！但是我们现在的pom呢，打包方式是为jar的方式，那么这种方式SpringBoot能不能来给我们写页面呢？当然是可以的，但是SpringBoot对于静态资源放置的位置，是有规定的！

   

2. 映射规则（源码已跳过）

   结论：以下四个目录的静态资源会被springboot识别到，优先级从上至下

   ```java
   "classpath:/META-INF/resources/"
   "classpath:/resources/"
   "classpath:/static/"
   "classpath:/public/"
   ```

3. 自定义静态资源路径

   我们也可以自己通过配置文件来指定一下，哪些文件夹是需要我们放静态资源文件的，在application.properties中配置；

   ```java
   
   spring.resources.static-locations=classpath:/coding/,classpath:/llq/
   ```

   

4. 首页处理（源码略过）

   结论：类路径下的三个目录中的任意index.html就是首页

## 4、注意点

在springboot中一般的静态页面在没有引入模板之前，在controller类方法中定义返回的html界面，通过相应的路径是跳转不过去的。必须得引入模板之后才能做接下来得工作







   

 



​      
