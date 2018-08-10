## SpringBoot简介

​	在传统的SSM框架进行项目开发前，需要先写大量的配置文件，有时导致功能需求小的项目使用SSM框架进行开发有些多此一举或事半功倍，反而降低了项目的开发进度。

​	并且，在不同和需求场景下，需要引用很多的第三方Jar包和Spring整合的兼容包，这么多的Jar包在很多时候很是令程序员头大，需要在Maven工程下一个一个的添加不同的dependency依赖。

​	那么SpringBoot的出现即就是将传统使用SSM开发项目程序员需要的写的一些配置文件，再项目的初始化时，统统由SpringBoot自动生成配置，大大减轻了程序员的劳动量。

​	而程序员在根据自己的项目场景配置不同的SpringBoot的开发场景，如我正在进行的web开发就配置SpringBoot的web开发场景，是普通的Java邮件开发就配置SpringBoot的邮件开发场景。

## 分析Spring工程

### **porm文件**

```xml
<parent>
<groupId>org.springframework.boot</groupId>
<artifactId>spring‐boot‐starter‐parent</artifactId>
<version>1.5.9.RELEASE</version>
</parent>
<dependencies>
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring‐boot‐starter‐web</artifactId>
</dependency>
</dependencies>
```

[^parent]: 该工程引用一个父工程*spring‐boot‐starter‐parent*

1. **查看spring‐boot‐starter‐parent的porm文件**

```xml
<?xml version="1.0" encoding="utf-8"?><project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-dependencies</artifactId>
        <version>2.0.3.RELEASE</version>
        <relativePath>../../spring-boot-dependencies</relativePath>
    </parent>
    <artifactId>spring-boot-starter-parent</artifactId>
    <packaging>pom</packaging>
    <name>Spring Boot Starter Parent</name>
    <description>Parent pom providing dependency and plugin management for applications
      built with Maven</description>
    <url>https://projects.spring.io/spring-boot/#/spring-boot-starter-parent</url>
    <properties>
		...
    </properties>
    <build>
		...
    </build>
</project>
```

​	该父工程主要也是依赖一个父工程*spring-boot-dependencies*

2. **查看spring-boot-dependencies的porm文件**

```xml
<?xml version="1.0" encoding="utf-8"?><project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-dependencies</artifactId>
    <version>2.0.3.RELEASE</version>
    <packaging>pom</packaging>
    <name>Spring Boot Dependencies</name>
    <description>Spring Boot Dependencies</description>
    <url>https://projects.spring.io/spring-boot/#</url>
		...
    <properties>
       核心部分
    </properties>
    <dependencyManagement>
        <dependencies>
            	核心部分
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-web</artifactId>
                <version>2.0.3.RELEASE</version>
            </dependency>
			.....
        </dependencies>
    </dependencyManagement>
    <build>
        <pluginManagement>
            <plugins>
                <plugin>
                   ...
                </plugin>
            </plugins>
        </pluginManagement>
    </build>
</project>

```

​	该工程才是该Maven工程依赖的最核心部分，该父工程的\<properties/\>声明了SpringBoot工程中依赖的所有Jar包版本

​	而\<dependencyManagement/\>明确定义了依赖Jar包的版本号（引用上面\<properties/\>中声明的版本号参数），但子工程并没有实际依赖，如果子工程需要依赖Jar包还需要在自己的porm文件中添加\<dependency/\>节点



[^dependencies]: 实际依赖父工程下了那个dependency工程依赖，根据项目工程的需求的开发场景，添加SpringBoot不同的开发场景，如Web开发场景，数据库开发场景等。。。

1. **查看*spring-boot-starter-web***

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd" xmlns="http://maven.apache.org/POM/4.0.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance
               
	核心部分               
  <dependencies>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter</artifactId>
      <version>2.0.3.RELEASE</version>
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-json</artifactId>
      <version>2.0.3.RELEASE</version>
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-tomcat</artifactId>
      <version>2.0.3.RELEASE</version>
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>org.hibernate.validator</groupId>
      <artifactId>hibernate-validator</artifactId>
      <version>6.0.10.Final</version>
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-web</artifactId>
      <version>5.0.7.RELEASE</version>
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.0.7.RELEASE</version>
      <scope>compile</scope>
    </dependency>
  </dependencies>
</project>
```

​	不同开发场景，实际在底层是根据场景的需求的Jar包，已经统一封装成了一个porm工程，在实际项目工程中，只要依赖了该场景，就相当于依赖了所有开发该场景的项目所需求的所有Jar包。

​	如：该Web场景中的dependency就已经帮我们依赖上了

  1. 数据验证所需的*hibernate-validator*
2. Web开发的*spring-web*
3. 即spring-webmvc

		在后期如果需要添加其他场景的话，可以直接配置spring-boot-starter--开头的依赖。

		如：想要使用Spring的AOP功能的话，就可以在\<dependencys/\>上添加

```xml
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-aop</artifactId>
                <version>2.0.3.RELEASE</version>
            </dependency>
```

### 启动类

​	SpringBoot的好处就在于不用程序员在手动大量的配置文件了。那么这些配置文件难道就真的在SpringBoot工程中不需要了吗？SpringBoot又是如何将创建这些大量的配置文件的？并且怎样将这些配置文件中的属性注入给了Spring中的组件？

​	其实，实际组件需要的配置一样都没少，只是SpringBoot都帮我们配置好了，它将一些常用的配置参数与一些主要的组件进行了绑定随后注入给了Spring容器。让我们在实际开发中感觉到不需要再进行一些繁琐的配置就可以使用到Spring框架中好用的功能。

​	通常，SpringBoot会在程序的主函数将整个项目进行初始化，如：自动配置组件并注入到Spring容器，扫描自定义组件并注入到容器中等。。。

> 主函数入口

```java
@SpringBootApplication
public class SpringbootApplication {

   public static void main(String[] args) {
      SpringApplication.run(SpringbootApplication.class, args);
   }
}
```

​	再SpringBoot应用中，该注解标注再哪个类上，就说明该类是SpringBoot的主配置类，SpringBoot就应该运行该类的main方法来启动SpringBoot应用。

> @SpringBootApplication注解

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(excludeFilters = {
@Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
@Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })
public @interface SpringBootApplication {
```



[^@SpringBootConfiguration]: 标注在某类上，表示这是一个SpringBoot的配置类

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Configuration
public @interface SpringBootConfiguration {
```

​	该注解底层实际就是Spring的@Configuration注解，用于声明配置类，但一个是SpringBoot的注解，一个是Spring的注解。这里仅仅只是做了一个区分而已。



[^@EnableAutoConfiguration]: 该注解是整个SpringBoot项目启动的核心，它在项目启动时，对整个项目进行了初始化。； 并自动配置场景所需的所有组件。

```
@AutoConfigurationPackage
@Import({AutoConfigurationImportSelector.class})
public @interface EnableAutoConfiguration {
```

1. **@AutoConfigurationPackage：**

   ```java
   @Target({ElementType.TYPE})
   @Retention(RetentionPolicy.RUNTIME)
   @Documented
   @Inherited
   @Import({Registrar.class})
   public @interface AutoConfigurationPackage {
   ```

   ​	Spring的底层注解@Import，给容器中导入一个组件Register.class。

   ​	该组件将主配置类（@SpringBootApplication标注的类）的所在包及下面所有子包里面的所有组件扫描到Spring容器。

2. **@Import({AutoConfigurationImportSelector.class})**

   ​	给容器添加一个AutoConfigurationImportSelector.class的组件。

   ​	该组件会扫描所有Jar包下的META-INF/spring.factories.properties文件，寻找一个属性名为EnableAutoConfiguration的所有值并返回一个List集合

   ​	该属性的每个值表示就是主要的核心组件，如：进行编码转换的HttpEncodingAutoConfiguration组件等.....

   ​	该组件会给容器中导入非常多的自动配置类（xxxAutoConfiguration）；就是给容器中导入这个场景需要的所有组件并配置好这些组件。

   ​	有了自动配置类，免去了我们手动编写配置注入功能组件等工作

   ​	但需要注意的是，虽然每个场景所需的组件都帮我们配置好了，但实际工程运行的时候，这些组件并不是都能够使用。每个组件在配置的时候都进行了条件判断，满足条件的情况下才会进行组件的参数配置以及将组件添加到容器中。如：

   ```java
   @Configuration
   @EnableConfigurationProperties({HttpEncodingProperties.class})
   @ConditionalOnWebApplication(
       type = Type.SERVLET
   )
   @ConditionalOnClass({CharacterEncodingFilter.class})
   @ConditionalOnProperty(
       prefix = "spring.http.encoding",
       value = {"enabled"},
       matchIfMissing = true
   )
   public class HttpEncodingAutoConfiguration {
       private final HttpEncodingProperties properties;
   
       public HttpEncodingAutoConfiguration(HttpEncodingProperties properties) {
           this.properties = properties;
       }
   
       @Bean
       @ConditionalOnMissingBean
       public CharacterEncodingFilter characterEncodingFilter() {
   ```

   ​	其中在类中或方法中的每一个@ConditionalOnXXX注解就是进行的一些条件判断。

   ​	必须是@Conditional指定的条件成立，才给容器中添加组件，配置配里面的所有内容才生效。

   | @Conditional扩展注解            | 作用（判断是否满足当前指定条件）                 |
   | ------------------------------- | ------------------------------------------------ |
   | @ConditionalOnJava              | 系统的java版本是否符合要求                       |
   | @ConditionalOnBean              | 容器中存在指定Bean；                             |
   | @ConditionalOnMissingBean       | 容器中不存在指定Bean；                           |
   | @ConditionalOnExpression        | 满足SpEL表达式指定                               |
   | @ConditionalOnClass             | 系统中有指定的类                                 |
   | @ConditionalOnMissingClass      | 系统中没有指定的类                               |
   | @ConditionalOnSingleCandidate   | 容器中只有一个指定的Bean，或者这个Bean是首选Bean |
   | @ConditionalOnProperty          | 系统中指定的属性是否有指定的值                   |
   | @ConditionalOnResource          | 类路径下是否存在指定资源文件                     |
   | @ConditionalOnWebApplication    | 当前是web环境                                    |
   | @ConditionalOnNotWebApplication | 当前不是web环境                                  |
   | @ConditionalOnJndi              | JNDI存在指定项                                   |

   ​	



## 配置文件

​	有时SpringBoot自动帮我们配置的一些参数可能不满足我的实际需求，这是可通过配置文件进行更改配置参数值，以达到自己的需求。

###内部配置文件加载孙顺序

​	SpringBoot启动时会扫描以下目录位置的一个**固定命名**的配置文件，application.properties或application.yml。

1. **项目根路径下的config文件目录**
2. **项目根目录**
3. **类路径下的config目录**
4. **类路径目录**

		优先级由高到低，高优先级的配置会覆盖低优先级的配置，SpringBoot会从这四个位置全部加载主配置文件，采用互补配置，并不是高优先级的配置文件会覆盖低优先级的配置文件，而是覆盖低优先级下的某个参数值。

###外部配置文件加载顺序

​	SpringBoot也可以从以下位置加载配置，优先级从高到低，高优先级的配置覆盖低优先级的配置，所有的配置同样会形成互补配置。

1. **命令行参数**

   所有的配置都可以在程序启动时在命令行上进行指定

   ```java
   java -jar 打包项目名.jar --server.port=8080 --server.context-path=/spring
   ```

   多个配置用空格分开，--配置项=值

   也可以通过配置参数指定配置文件路径，从而一次性配置多个参数

   ```java
   java -jar 打包项目名.jar --spring.config.location=目录路径/application.properties
   ```

2. **由Jar包外向Jar包内进行寻在**

3. **@Configuration注解类上的@PropertySource**

4. 。。。

   所有支持的配置加载来源更多查看[官方文档](https://docs.spring.io/spring-boot/docs/1.5.9.RELEASE/reference/htmlsingle/#boot-features-external-config)

### YAML语法

	#### 前言

​	以前的配置文件，大多都是用的是xxx.xml文件；

​	而YAML以数据为中心，比JSON，XML等更适合做配置文件

**YAML配置例子**

```yaml
server:
  port: 8080
  path: /spring
```

**XML配置例子**

```xml
<server>
	<prot>
    	8080
    </prot>
    <path>
    	/spring
    </path>
</server>
```

#### 基本语法

​	K:(空格)V：表示一对键值对（**空格必须有**）

​	以空格的缩进来控制层级关系，只要是左对齐的一列数据，都是同一层级的

#### 值的写法

- 字面量：普通的值（数字，字符串，布尔）

  ```yaml
  test:
  	int: 0
  	boolean: true
  	String: Hello SpringBoot
  ```

  字符串默认不用加单引号或者双引号，但可根据实际需求可自行添加。

  ""：双引号，不会转义字符串里面的特殊字符，特殊字符会作为本身想表达的意思

  ```yam
  test:
  	String: "Hello\nSpringBoot"
  ```

  输出：Hello\nSpringBoot

  ''：单引号，会转义特殊字符，特殊符号最终只是一个普通的字符串数据

  ```yaml
  test:
  	String: 'Hello\nSpringBoot'
  ```

  输出：Hello SpringBoot

- 对象、Map。同时属性和值，所以语法都一样

  1. K：V方式

  ```yaml
  域属性名或Map属性名：
  	域属性成员名或Map的Key名：值
  	域属性成员名或Map的Key名：值
  ```

  2. 行内写法

  ```yaml
  类名: {属性名：值，属性名：值}
  ```

- 数组（List、Set）

  ```
  数组名：
  	- 值
  	- 值
  ```

#### @Value获取值与@ConfigurationProperties获取值比较

|                      | @ConfigurationProperties | @Value     |
| -------------------- | ------------------------ | ---------- |
| 功能                 | 批量注入配置文件中的属性 | 一个个指定 |
| 松散绑定（松散语法） | 支持                     | 不支持     |
| SpEL                 | 不支持                   | 支持       |
| JSR303数据校验       | 支持                     | 不支持     |
| 复杂类型封装         | 支持                     | 不支持     |

​	如果说，我们只是在某个业务逻辑中获取一个配置文件中的某项值，则使用@Value

​	如果说，我们专门编写一个javaBean来配置文件进行映射，我们就直接使用@ConfigurationProperties；



### @PropertySource

​	配置类中指定加载的配置文件； 

​	需要注意的是，如果指定的是YAML文件的话，其文件名只能是applicaiton，否则无法对其进行注入

### @ImportResource&@Bean 

​	SpringBoot里面没有Spring的配置文件，我们自己编写的配置文件，也不能自动识别，想让Spring的配置文件生效，加载进来，需要将@ImportResource标注到配置类上。

```java
@ImportResource(locations = "classpath:beans.xml")//导入自定义Spring配置文件
@SpringBootApplication
public class SpringbootApplication {

   public static void main(String[] args) {
      SpringApplication.run(SpringbootApplication.class, args);

   }
}
```




## SpringBoot的Web开发

### 简介

1）、创建SpringBoot应用，选中我们需要的模块； 

2）、SpringBoot已经默认将这些场景配置好了，只需要在配置文件中指定少量配置就可以运行起来 

3）、自己编写业务代码； 



### SpringBoot对静态资源的映射规则 

1. **所有 /webjars/** ，都去 classpath:/META-INF/resources/webjars/ 找资源；** 

   ![webjars资源映射路径](E:\笔记\Java框架\photo\webjars资源映射路径.bmp)

   [Web开发常用的框架Jars下载途径](http://www.webjars.org/)

   ```xml
   <dependency>
       <groupId>org.webjars</groupId>
       <artifactId>jquery</artifactId>
       <version>3.3.1</version>
   </dependency>
   ```

   [^前端中映入资源]: <script src="/webjars/jquery/3.3.1/jquery.js" />

   

2. **"/**" 访问当前项目的任何资源，都去（静态资源的文件夹）找映射** 

   - "classpath:/META‐INF/resources/", 
   - "classpath:/resources/",
   - "classpath:/static/",
   -  "classpath:/public/"
   - "/"：当前项目的根路径 

   ![静态资源映射路径](E:\笔记\Java框架\photo\静态资源映射路径.bmp)

   如：localhost:8080/abc === 去静态资源文件夹里面找abc

   

3. **欢迎页； 静态资源文件夹下的所有index.html页面；被"/**"映射；** 

   如：localhost:8080/ 找index页面 



4. **所有的 **/favicon.ico 都是在静态资源文件下找； 如向定制页面标签logo**



### 模板引擎

####前言

​	在JavaWeb开发中常用的模板引擎一般有JSP、Velocity、Freemarker、Thymeleaf。

但所有的模板引擎的原理大多都一样，混有原生的HTML标签和模板引擎语言标签经过模板引擎解析之后就是填充了数据的、能被浏览器解析的HTML语言文件。

![模板引擎原理](E:\笔记\Java框架\photo\模板引擎原理.bmp)

​	SpringBoot推荐的Thymeleaf，因为该模板语言的语法更简单，功能更强大。

#### 引入Thymeleaf

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring‐boot‐starter‐thymeleaf</artifactId>
    版本由启动器确定
</dependency>
```

手动切换Thymeleaf版本

```xml
<properties>
    <thymeleaf.version>3.0.9.RELEASE</thymeleaf.version>
    <!‐‐ 布局功能的支持程序 thymeleaf3主程序 layout2以上版本 ‐‐>
    <!‐‐ thymeleaf2 layout1‐‐>
    <thymeleaf‐layout‐dialect.version>2.2.2
    </thymeleaf‐layout‐dialect.version>
</properties>
```

#### Thymeleaf使用

​	只要我们把HTML页面放在classpath:/templates/，thymeleaf就能自动渲染。

1. **在HTML文件中导入thymeleaf的名称空间**

   ```xml
   <html lang="en" xmlns:th="http://www.thymeleaf.org">
   ```

   

   


##### 外部化⽂本的存放问题

​	SpringBoot的自动化配置默认会寻找类路径下的messages消息文件。

```java
@ConfigurationProperties(prefix = "spring.messages")
public class MessageSourceAutoConfiguration {
/**
* Comma‐separated list of basenames (essentially a fully‐qualified classpath
* location), each following the ResourceBundle convention with relaxed support for
* slash based locations. If it doesn't contain a package qualifier (such as
* "org.mypackage"), it will be resolved from the classpath root.
*/
private String basename = "messages";
//我们的配置文件可以直接放在类路径下叫messages.properties；
@Bean
public MessageSource messageSource() {
ResourceBundleMessageSource messageSource = new ResourceBundleMessageSource();
if (StringUtils.hasText(this.basename)) {
//设置国际化资源文件的基础名（去掉语言国家代码的）
messageSource.setBasenames(StringUtils.commaDelimitedListToStringArray(
StringUtils.trimAllWhitespace(this.basename)));
}
if (this.encoding != null) {
messageSource.setDefaultEncoding(this.encoding.name());
}
messageSource.setFallbackToSystemLocale(this.fallbackToSystemLocale);
messageSource.setCacheSeconds(this.cacheSeconds);
messageSource.setAlwaysUseMessageFormat(this.alwaysUseMessageFormat);
return messageSource;
}
```

​	如果没有手动配置指定消息文件路径而消息文件的命名不为message.properties，那么前端显示将出现??home.welcome_zh_CN??的现象。

​	解决办法：

1. 修改消息文件名为默认的messages.properties

2. 在配置文件中指定消息文件路径

   ```yaml
   spring:
     messages:
       basename: 目录名/消息文件名
   ```

   