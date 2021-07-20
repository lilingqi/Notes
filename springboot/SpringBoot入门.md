# 1、SpringBoot介绍



## 1.1 springboot简介

​     SpringBoot 是一个快速开发的框架, 封装了Maven常用依赖、能够快速的整合第三方框架；简化XML配置，全部采用注解形式，内置Tomcat、Jetty、Undertow，帮助开发者能够实现快速开发，SpringBoot的Web组件 默认集成的是SpringMVC框架。sprinfboot可以让大家更容易的使用 Spring 、更容易的集成各种常用的中间件、开源软件；

​     简单来说就是SpringBoot其实不是什么新的框架，它默认配置了很多框架的使用方式，就像maven整合了所有的jar包，spring boot整合了所有的框架 ，其次==**springboot是在spring的基础上搭设的框架**==。Spirng Boot 本身并不提供 Spring 框架的核心特性以及扩展功能，只是用于快速、敏捷地开发新一代基于 Spring 框架的应用程序。也就是说，它并不是用来替代 Spring 的解决方案，而是和 Spring 框架紧密结合用于提升 Spring 开发者体验的工具。Spring Boot 以**约定大于配置的核心思想**，默认帮我们进行了很多设置，多数 Spring Boot 应用只需要很少的 Spring 配置。同时它集成了大量常用的第三方库配置（例如 Redis、MongoDB、Jpa、RabbitMQ、Quartz 等等），Spring Boot 应用中这些第三方库几乎可以零配置的开箱即用

## 1.2 SpringBoot原理介绍：

1. 能够帮助开发者实现快速整合第三方框架 （原理：Maven依赖封装）

2. 去除xml配置（并没又完全去除） 完全采用注解化 （原理：Spring体系中内置注解方式）

3. 无需外部Tomcat、内部实现服务器（原理：Java语言支持内嵌入Tomcat服务器）

## 1.3 SpringBoot和SpringMVC区别

   SpringBoot 是一个快速开发的框架,能够快速的整合第三方框架，简化XML配置，全部采用注解形式，内置Tomcat容器,帮助开发者能够实现快速开发，SpringBoot的Web组件 默认集成的是SpringMVC框架。

   SpringMVC是控制层。

## 1.4 SpringBoot和SpringCloud区别

   SpringBoot 是一个快速开发的框架,能够快速的整合第三方框架，简化XML配置，全部采用注解形式，内置Tomcat容器,帮助开发者能够实现快速开发，SpringBoot的Web组件 默认集成的是SpringMVC框架。

SpringMVC是控制层。

​     SpringCloud依赖与SpringBoot组件，使用SpringMVC编写Http协议接口，同时SpringCloud是一套完整的微服务解决框架。

#  2、SpringBoot快速入门

##  2.1 直接使用IDEA来创建工程

1、创建一个新项目

2、选择spring initalizr ， 可以看到默认就是去官网的快速构建工具那里实现

3、填写项目信息

4、选择初始化的组件（初学勾选 Web 即可,这个web其实就是集成了springmvc）

5、填写项目路径

6、等待项目构建成功

==可能会出现的问题：如下图创建工程的时候，采用默认网址可能会创建不成功，可以自己采用另一个来创建==

![image-20210719220641844](C:\Users\李令齐\AppData\Roaming\Typora\typora-user-images\image-20210719220641844.png)

## 2.2 项目结构分析

通过上面步骤完成了基础项目的创建。就会自动生成以下文件。

1、程序的主启动类

2、一个 application.properties 配置文件

3、一个 测试类

4、一个 pom.xml

### 2.1 分析pom.xml

```java
<!-- 父依赖
    父依赖的作用：它可以提供dependency management,也就是说依赖管理，引入以后在申明其它dependency的时候就不需要version
 -->
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.2.5.RELEASE</version>
    <relativePath/>
</parent>

<dependencies>
    <!-- web场景启动器  初始化勾选的web组件就是导入了这个依赖-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <!-- springboot单元测试 -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
        <!-- 剔除依赖 -->
        <exclusions>
            <exclusion>
                <groupId>org.junit.vintage</groupId>
                <artifactId>junit-vintage-engine</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
</dependencies>

<build>
    <plugins>
        <!-- 打包插件 -->
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

### 2.2 分析主程序类

在分析主程序类之前我们先来分析几个重要的注解和启动方式

1. @RestController ：在类上加上RestController表示修饰该Controller所有的方法返回JSON格式,直接可以编写Restful风格接口

注意该注解是SpringMVC提供的哦！简单来说就是不用在需要返回json格式的时候在方法上面家@RequestBody注解。

2. @EnableAutoConfiguration：作用在于让 Spring Boot  根据应用所声明的依赖来对 Spring 框架进行自动配置这个注解告诉Spring Boot根据添加的jar依赖猜测你想如何配置Spring。由于spring-boot-starter-web添加了Tomcat和Spring MVC，所以auto-configuration将假定你正在开发一个web应用并相应地对Spring进行设置。
3. @SpringBootApplication：@SpringBootApplication 被 @Configuration、@EnableAutoConfiguration、@ComponentScan（扫描有controller注解的类） 注解所修饰，换言之 Springboot 提供了这个注解来替代以上三个注解

启动方式一：

```java
@RestController
@EnableAutoConfiguration //使得程序以web方式启动
public class HelloController {
	@RequestMapping("/hello")
	public String index() {
		return "Hello World";
	}	
public static void main(String[] args) {
		SpringApplication.run(HelloController.class, args);
	}
}

```

启动方式二：

```java
@ComponentScan(basePackages = "com.mayikt.controller")---控制器扫包范围
@ComponentScan(basePackages = "com.mayikt.controller") //启动方式一因为main方法在同个类下所以不需要包扫描
@EnableAutoConfiguration 
public class App {
	public static void main(String[] args) {
		SpringApplication.run(App.class, args);
	}
}

```

启动方式三：

```java
@SpringBootApplication //这个注解= @ComponetScan + @EnableAutoConfiguration + @Configuration
public class SpringbootMybatisApplication {

	public static void main(String[] args) {
		SpringApplication.run(SpringbootMybatisApplication.class, args);
	}

}
```



==我们平时在创建项目的时候，就会有一个如启动方式三的主程序类。但是要注意我们在创建controller类的时候一定要放在跟主程序类的同一个包下或者同一个包的子包下。这样SpringBootApplication注解才可以扫描到。==

# 3、web开发

## 3.1 静态资源的访问

在我们进行web应用开发的时候，需要引用大量的js、css、图片等静态资源springboot默认帮我们放在类路径（classpath）下的：

/static

/public

/resources     

/META-INF/resources

**举例：我们可以在src/main/resources/目录下创建static，在该位置放置一个图片文件。启动程序后，尝试访问http://localhost:8080/D.jpg。如能显示图片，配置成功。**

## 3.2 模板引擎（Thymeleaf）

这部分内容已经单独做了笔记（因为springboo**一般**是不支持jsp的所以需要用到这个）

## 3.3 yml 和 properties

SpringBoot支持两种配置方式,一种是properties文件,一种是yml。

使用yml可以减少配置文件的重复性。

例如：

```java
// properties
llq.name=llq
llq.age=23
```

```java
//yml
llq: 
 name: llq
 age: 23
```

==注意点：一般企业开发中yml用的比较多。然后就是在使用yml的时候注意空格和缩进。==

# 4、数据库的访问

## 4.1 SpringBoot整合JdbcTemplate

### 4.1.1 pom文件的引用

```java
<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.1.8.RELEASE </version>
	</parent>
	<dependencies>
       // 新增jdbc相关依赖
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-jdbc</artifactId>
		</dependency>
      // 新增mysql依赖
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<version>5.1.21</version>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
	</dependencies>

```

### 4.1.2 application.yml新增配置

```java
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/mybatis
    username: root
    password: 123456
    driver-class-name: com.mysql.jdbc.Driver

```

### 4.1.3 Account类和AccountController类

```java
package com.llq.pojo;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

/**
 * @author llq
 * @create 2021-07-19  23:27
 */
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Account {
    private Integer ID;
    private Integer UID;
    private Double MONEY;

}
```



```java
@Controller
public class AcoountController {
    //配置了数据源，JdbcTemplate对象就已经被装进spring容器中了
    @Autowired
    JdbcTemplate jdbcTemplate;
    @RequestMapping("/findAll")
    public String findAllById(Model model){
        List<Map<String, Object>> accounts = jdbcTemplate.queryForList("select * from account");
        model.addAttribute("accounts",accounts);
        return "success";
    }
}
```

## 4.2 SpringBoot整合Mybatis（纯注解）

### 4.2.1 pom文件引入

```java
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.1.8.RELEASE</version>
</parent>
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <!-- springboot 整合mybatis -->
    <dependency>
        <groupId>org.mybatis.spring.boot</groupId>
        <artifactId>mybatis-spring-boot-starter</artifactId>
        <version>1.1.1</version>
    </dependency>
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>5.1.21</version>
    </dependency>

</dependencies>

```



### 4.2.2 编辑application.yml

```java
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/mybatis
    username: root
    password: 123456
    driver-class-name: com.mysql.jdbc.Driver
```

### 4.2.3 Mapper(Dao)代码

```java
@Repository
@Mapper//一种方式是在这个位置加Mapper注解
public interface UserMapper {
	@Select("SELECT * FROM USERS WHERE NAME = #{name}")
	User findByName(@Param("name") String name);
	@Insert("INSERT INTO USERS(NAME, AGE) VALUES(#{name}, #{age})")
	int insert(@Param("name") String name, @Param("age") Integer age);

```

==@Mapper注解：从mybatis3.4.0开始加入了@Mapper注解，目的就是为了不再写mapper映射文件，添加了@Mapper注解之后这个接口在编译时会生成相应的实现类，需要注意的是：这个接口中不可以定义同名的方法，因为会生成相同的id 也就是说这个接口是不支持重载的。==

==@Mapper注解的的作用==

1:为了把mapper这个DAO交給Spring管理 http://412887952-qq-com.iteye.com/blog/2392672 

2:为了不再写mapper映射文件 https://blog.csdn.net/weixin_39666581/article/details/103899495

3:为了给mapper接口 自动根据一个添加@Mapper注解的接口生成一个实现类 http://www.tianshouzhi.com/api/tutorials/mapstruct/292 ==

### 4.2.4 启动方式以及Controller层代码

```java
@SpringBootApplication
//@MapperScan("com.llq.mapper") 另外一种方式就是在主程序启动类上加上这个扫描注解。两种方式二选一
public class AppMybatis {
    public static void main(String[] args) {
        SpringApplication.run(AppMybatis.class);
    }
}
@RestController
public class UserController {
    @Autowired
    UserMapper userMapper;
    @Autowired
    AccountMapper accountMapper;

    @RequestMapping("/findAll")
    public List<User> findAll() {
        return userMapper.findAll();
    }
```

## 4.3 SpringBoot整合Mybatis（xml）

### 4.3.1pom文件引用

同上

### 4.3.2 编辑application.yml

```java
spring:
 datasource:
     username: root
     password: 123456
     url: jdbc:mysql://localhost:3306/mybatis
     driver-class-name: com.mysql.jdbc.Driver
         
mybatis:
  type-aliases-package: com.llq.pojo //给实体类取别名
  mapper-location: classpath:mybatis/mapper/*.xml //指定当前配置文件所在位置

```

### 4.3.3 Mapper.xml配置

```java
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.llq.mapper.AccountMapper">

    <select id="findAll" resultType="Account">
        select * from account;
    </select>

    <!--<select id="findAccountById" resultType="Account" parameterType="int">-->
        <!--select * from account where id = #{id};-->
    <!--</select>-->

</mapper>
```

### 4.3.4 Mapper（Dao）层程序

```java
package com.llq.dao;
import com.llq.pojo.Account;
import org.apache.ibatis.annotations.Mapper;
import org.springframework.stereotype.Repository;
import java.util.List;

/**
 * @author llq
 * @create 2021-07-20  9:13
 */
@Mapper
@Repository
public interface AccountDao {
    List<Account> findAll();
}

```

### 4.4.4 Controller层程序

```java
package com.llq.controller;
import com.llq.dao.AccountDao;
import com.llq.pojo.Account;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import java.util.List;
/**
 * @author llq
 * @create 2021-07-20  9:14
 */
@RestController
public class AccountController {
    @Autowired
    AccountDao accountDao;
    @RequestMapping("/findAll")
    public List<Account> findAll(){
        return accountDao.findAll();
    }
}

```

==注：这个xml方式整合的程序我没有跑成功，问题有待解决==

总结：xml方式和纯注解方式的区别主要是在第二步和第三步，xml方式要多写一个Mapper.xml文件同时在application.yml文件中要指定配置文件所在位置才可整合成功。xml方式开发更加有利于sql语句的灵活使用

## 4.4 SprngBoot整合多数据源

### 4.4.1 配置文件中新增两个数据源

  ```java
  spring:
    datasource:
      ###会员数据库
      member:
        jdbc-url: jdbc:mysql://localhost:3306/user
        username: root
        password: root
        driver-class-name: com.mysql.jdbc.Driver
      ###订单数据库
      order:
        jdbc-url: jdbc:mysql://localhost:3306/order
        username: root
        password: root
        driver-class-name: com.mysql.jdbc.Driver
  
  ```

注：如果是SpringBoot2配置多数据源 ，报如下错误：

“jdbcUrl is required with driverClassName.”或者Cause: java.lang.IllegalArgumentException: dataSource or dataSourceClassName or jdbcUrl is required.] with root cause

解决方案：

spring.datasource.url 和spring.datasource.driverClassName，换成

spring.datasource.jdbc-url和spring.datasource.driver-class-name

### 4.4.2 数据库数据源相关配置

会员数据源

```java
@Configuration
@MapperScan(basePackages = "com.mayikt.member.mapper", sqlSessionFactoryRef = "memberSqlSessionFactory")
public class MemberDataSourceConfig {

    /**
     * 将会员db注册到容器中
     *
     * @return
     */
    @Bean(name = "memberDataSource")
    @ConfigurationProperties(prefix = "spring.datasource.member")
    public DataSource memberDataSource() {
        return DataSourceBuilder.create().build();
    }

    /**
     * 将会员SqlSessionFactory注册到容器中
     *
     * @param dataSource
     * @return
     * @throws Exception
     */
    @Bean(name = "memberSqlSessionFactory")
    public SqlSessionFactory memberSqlSessionFactory(@Qualifier("memberDataSource") DataSource dataSource) throws Exception {
        SqlSessionFactoryBean sqlSessionFactoryBean = new SqlSessionFactoryBean();
        sqlSessionFactoryBean.setDataSource(memberDataSource());
        return sqlSessionFactoryBean.getObject();
    }

    /**
     * 创建会员管理器
     *
     * @param dataSource
     * @return
     */
    @Bean(name = "memberTransactionManager")
    public DataSourceTransactionManager memberTransactionManager(@Qualifier("memberDataSource") DataSource dataSource) {
        return new DataSourceTransactionManager(dataSource);
    }

    /**
     * 创建订单sqlSesion模版
     *
     * @param sqlSessionFactory
     * @return
     * @throws Exception
     */
    @Bean(name = "memberSqlSessionTemplate")
    public SqlSessionTemplate menberSqlSessionTemplate(
            @Qualifier("memberSqlSessionFactory") SqlSessionFactory sqlSessionFactory) throws Exception {
        return new SqlSessionTemplate(sqlSessionFactory);
    }
}

```

订单数据源

``` java
@Configuration
@MapperScan(basePackages = "com.mayikt.order.mapper", sqlSessionFactoryRef = "orderSqlSessionFactory")
public class OrderDataSourceConfig {

    /**
     * 将订单db注册到容器中
     *
     * @return
     */
    @Bean(name = "orderDataSource")
    @ConfigurationProperties(prefix = "spring.datasource.order")
    public DataSource orderDataSource() {
        return DataSourceBuilder.create().build();
    }

    /**
     * 将订单SqlSessionFactory注册到容器中
     *
     * @param dataSource
     * @return
     * @throws Exception
     */
    @Bean(name = "orderSqlSessionFactory")
    public SqlSessionFactory orderSqlSessionFactory(@Qualifier("orderDataSource") DataSource dataSource) throws Exception {
        SqlSessionFactoryBean sqlSessionFactoryBean = new SqlSessionFactoryBean();
        sqlSessionFactoryBean.setDataSource(orderDataSource());
        return sqlSessionFactoryBean.getObject();
    }

    /**
     * 创建订单管理器
     *
     * @param dataSource
     * @return
     */
    @Bean(name = "orderTransactionManager")
    public DataSourceTransactionManager orderTransactionManager(@Qualifier("orderDataSource") DataSource dataSource) {
        return new DataSourceTransactionManager(dataSource);
    }

    /**
     * 创建订单sqlSesion模版
     *
     * @param sqlSessionFactory
     * @return
     * @throws Exception
     */
    @Bean(name = "orderSqlSessionTemplate")
    public SqlSessionTemplate menberSqlSessionTemplate(
            @Qualifier("orderSqlSessionFactory") SqlSessionFactory sqlSessionFactory) throws Exception {
        return new SqlSessionTemplate(sqlSessionFactory);
    }
}

```

### 4.4.3 创建分包Mapper（Dao）

会员Mapper

```java
public interface MemberMapper {
    @Insert("insert into users values(null,#{name},#{age});")
    public int addUser(@Param("name") String name, @Param("age") Integer age);
}
```

订单Mapper

```java
public interface OrderMapper {
    @Insert("insert into order_number values(null,#{number});")
    int inserOrder(@Param("number") String number);
}
```

### 4.4.4 pom文件

```
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.1.8.RELEASE</version>
</parent>
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <!-- springboot 整合mybatis -->
    <dependency>
        <groupId>org.mybatis.spring.boot</groupId>
        <artifactId>mybatis-spring-boot-starter</artifactId>
        <version>1.1.1</version>
    </dependency>
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>5.1.21</version>
    </dependency>

</dependencies>

```

### 4.4.5 数据库表结构

```java
CREATE TABLE `users` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(32) NOT NULL COMMENT '用户名称',
  `age` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=5 DEFAULT CHARSET=utf8;


CREATE TABLE `order_number` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `order_name` varchar(255) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=5 DEFAULT CHARSET=utf8;

```

# 5、SpringBoot中的事务管理

## 5.1 SpringBoot整合事务管理

**==springboot默认集成事务,只主要在方法上加上@Transactional即可==**

# 6、SpringBoot整合热部署

## 6.1 Spring Boot集成lombok让代码更简洁

1.需要安装Idea整合 整合Lombok插件，

![image-20210720094905313](C:\Users\李令齐\AppData\Roaming\Typora\typora-user-images\image-20210720094905313.png)

2. 添加lombok依赖

   ```java
   <dependency>
   	<groupId>org.projectlombok</groupId>
   	<artifactId>lombok</artifactId>
   </dependency>
   ```

### 6.1.1 lombok可以提供的注解

```tex
@Data 标签，生成getter/setter toString()等方法 
@NonNull : 让你不在担忧并且爱上NullPointerException 
@CleanUp : 自动资源管理：不用再在finally中添加资源的close方法 
@Setter/@Getter : 自动生成set和get方法 
@ToString : 自动生成toString方法 
@EqualsAndHashcode : 从对象的字段中生成hashCode和equals的实现 
@NoArgsConstructor/@RequiredArgsConstructor/@AllArgsConstructor 
自动生成构造方法 
@Data : 自动生成set/get方法，toString方法，equals方法，hashCode方法，不带参数的构造方法 
@Value : 用于注解final类 
@Builder : 产生复杂的构建器api类 
@SneakyThrows : 异常处理（谨慎使用） 
@Synchronized : 同步方法安全的转化 
@Getter(lazy=true) : 
@Log : 支持各种logger对象，使用时用对应的注解，如：@Log4

```

6.1.2 打印日志

```tex
private static Logger log = Logger.getLogger(App.class);
直接在类上加上@Slf4j 
```

## 6.2 SpringBoot整合热部署框架

### 6.2.1 什么是热部署

==修改java类或页面或者静态文件，不需要手动重启==

原理：类加载器 

适合于本地开发环境

### 6.2.2 Maven依赖

```java
<!--SpringBoot热部署配置 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <scope>runtime</scope>
    <optional>true</optional>
</dependency>
```

### 6.2.3 IDEA工具设置

![image-20210720100240593](C:\Users\李令齐\AppData\Roaming\Typora\typora-user-images\image-20210720100240593.png)

![image-20210720100255031](C:\Users\李令齐\AppData\Roaming\Typora\typora-user-images\image-20210720100255031.png)

# 7、整合配置文件

1. 在springboot整合配置文件，分成两大类：

application.properties

application.yml

或者是

Bootstrap.properties

Bootstrap.yml

相对于来说yml文件格式写法更加精简，减少配置文件的冗余性。

2. 加载顺序：

bootstrap.yml 先加载 application.yml后加载

bootstrap.yml 用于应用程序上下文的引导阶段。

bootstrap.yml 由父Spring ApplicationContext加载。

3. 区别：

bootstrap.yml 和 application.yml 都可以用来配置参数。

bootstrap.yml 用来程序引导时执行，应用于更加早期配置信息读取。可以理解成系统级别的一些参数配置，这些参数一般是不会变动的。一旦bootStrap.yml 被加载，则内容不会被覆盖。

application.yml 可以用来定义应用级别的， 应用程序特有配置信息，可以用来配置后续各个模块中需使用的公共参数等。

分布式配置中心：

## 7.1 使用@value取配置文件的值

```java
@Value("${llq.name}")
private String name;
```

## 7.2 使用@ConfigurationProperties取配置文件的值

1. 导入maven依赖

   ```java
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-configuration-processor</artifactId>
       <optional>true</optional>
   </dependency>
   ```

2. 测试代码

   ```java
   import org.springframework.boot.context.properties.ConfigurationProperties;
   import org.springframework.stereotype.Component;
   @Component
   @ConfigurationProperties(prefix = "llq")
   public class MayiktUserEntity {
       private String addres;
       private String age;
       private String name;
       public String getAddres() {
           return addres;
       }
       public String getAge() {
           return age;
       }
       public String getName() {
           return name;
       }
       public void setAddres(String addres) {
           this.addres = addres;
       }
       public void setAge(String age) {
           this.age = age;
       }
       public void setName(String name) {
           this.name = name;
       }
       @Override
       public String toString() {
           return "MayiktUserEntity{" +
                   "addres='" + addres + '\'' +
                   ", age='" + age + '\'' +
                   ", name='" + name + '\'' +
                   '}';
       }
   }
   
   ```

   ```java
   //application. yml文件
   llq:
     addres: www.llq.com
     age: 22
     name: llq
   
   ```

   ```java
   @Autowired
   private MayiktUserEntity mayiktUserEntity;
   @RequestMapping("/getNameAndAgeAddres")
   public String getNameAndAgeAddres() {
       return mayiktUserEntity.toString();
   }
   ```

## 7.3 多环境配置

```java
//选定开发环境
spring:
  profiles:
    active: pre
```

```java
//一般配置环境采用此种命名方法
application-dev.yml：开发环境
application-test.yml：测试环境
application-prd.yml：生产环境

```



## ## 7.4 其他重要配置

```java
server:
  port: 8081
  servlet:
    context-path: /llq   //采取localhost：8080/llq才能访问
Springboot 默认的情况下整合tomcat容器
```

