## JPA前言

### 介绍

	JPA 的始作俑者就是 Hibernate 的作者，–Hibernate 从 3.2 开始兼容 JPA 
	
	JPA本质上是一种ORM规范，不是ORM框架，类似于JDBC和Mysql的JDBC驱动。
	
	因为JPA并未提供ORM实现，它只是制定了一些规范，提供了一些编程的API接口，但具体实现则由ORM厂商提供实现，现在流行的实现产品有Hibernate 3.2、TopLink 10.1、OpenJPA都提供了实现JPA的实现。

### 优势

- **标准化**

  	提供相同的API，这保证了基于JPA开发的企业应用能够经过少量的修改就能够在不同的JPA框架下运行

- **简单易用，集成方便**

  	JPA的主要目标之一就是提供简单的编程模型，在JPA框架下创建实体和创建Java类一样简单，只需要使用Javax.persistence.Entity进行注释，JPA的框架和接口也都非常简单

- **可媲美JDBC的查询能力**

  	JPA的查询语言是面向对象的，JPA定义了独特的JPQL，而且能够支持批量更新和修改、JOIN、GROUNP BY、HAVING等通常只有SQL才能够提供的高级查询特性，甚至还能够支持子查询

- **支持面向对象的高级特性**

  	JPA中能够支持面向对象的高级特性，如类之间的继承、多态和类之间的复杂关系、最大限度的使用面向对象的模型



### 三大核心技术

- **ORM映射元数据**

  	JPA支持XML和JDK5.0注解两种元数据的形式，元数据描述对象和表之间的映射关系，框架据此将实体对象持续化到数据表中

- **JPA的API**

  	用来操作实体对象，执行CRUD操作，框架在后台完成所有的事情，开发者从繁琐的JDBC和SQL代码中解脱出来

- **JPQL（查询语言）**

  	这是持续化操作中很重要的一个方面，通过面向对象而非面向数据库的查询语言查询数据，避免程序和具体的SQL紧密耦合




## 使用步骤

1. **导入相关重要Jar包**

   - mysql的jdbc驱动

     ```
             <!--mysql的jdbc驱动-->
             <dependency>
                 <groupId>mysql</groupId>
                 <artifactId>mysql-connector-java</artifactId>
                 <version>6.0.6</version>
             </dependency>
     ```

   - JPA规范的jar包 

     ```
             <dependency>
                 <groupId>org.hibernate.javax.persistence</groupId>
                 <artifactId>hibernate-jpa-2.0-api</artifactId>
                 <version>1.0.1.Final</version>
             </dependency>
     ```

   - Hibernate核心包 

     ```
             <dependency>
                 <groupId>org.hibernate</groupId>
                 <artifactId>hibernate-core</artifactId>
                 <version>4.3.11.Final</version>
             </dependency>
     ```

   - Hibernate针对JPA的实现包 

     ```
             <dependency>
                 <groupId>org.hibernate</groupId>
                 <artifactId>hibernate-entitymanager</artifactId>
                 <version>4.3.11.Final</version>
             </dependency>
     ```

   - ehcache的二级缓存框架

   - slf4j的日志框架





1. **创建persistence.xml文件，在这个文件中配置持续化单元**

   - 需要指定哪个数据库进行交互
   - 指定JPA使用哪个持久化的框架以及配置该框架的基本属性

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <persistence version="2.0"
                xmlns="http://java.sun.com/xml/ns/persistence" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                xsi:schemaLocation="http://java.sun.com/xml/ns/persistence http://java.sun.com/xml/ns/persistence/persistence_2_0.xsd">
       <persistence-unit name="jpa-1" transaction-type="RESOURCE_LOCAL">
           <!--
           配置使用什么 ORM 产品来作为 JPA 的实现
           1. 实际上配置的是  javax.persistence.spi.PersistenceProvider 接口的实现类
           2. 若 JPA 项目中只有一个 JPA 的实现产品, 则也可以不配置该节点.
           -->
           <provider>org.hibernate.ejb.HibernatePersistence</provider>
   
           <!-- 添加持久化类 -->
           <class>data.ClazzEntity</class>
           <class>data.StudentEntity</class>
           <class>data.TeacherEntity</class>
   
   
           <!--
           配置二级缓存的策略
           ALL：所有的实体类都被缓存
           NONE：所有的实体类都不被缓存.
           ENABLE_SELECTIVE：标识 @Cacheable(true) 注解的实体类将被缓存
           DISABLE_SELECTIVE：缓存除标识 @Cacheable(false) 以外的所有实体类
           UNSPECIFIED：默认值，JPA 产品默认值将被使用
           -->
           <shared-cache-mode>ENABLE_SELECTIVE</shared-cache-mode>
   
           <properties>
               <!-- 连接数据库的基本信息 -->
               <property name="javax.persistence.jdbc.driver" value="com.mysql.jdbc.Driver"/>
               <property name="javax.persistence.jdbc.url" value="jdbc:mysql:///test"/>
               <property name="javax.persistence.jdbc.user" value="root"/>
               <property name="javax.persistence.jdbc.password" value="123456"/>
   
               <!-- 配置 JPA 实现产品的基本属性. 配置 hibernate 的基本属性 -->
               <property name="hibernate.format_sql" value="true"/>
               <property name="hibernate.show_sql" value="true"/>
               <property name="hibernate.hbm2ddl.auto" value="update"/>
   
               <!-- 二级缓存相关 -->
               <property name="hibernate.cache.use_second_level_cache" value="true"/>
               <property name="hibernate.cache.region.factory_class"
                         value="org.hibernate.cache.ehcache.EhCacheRegionFactory"/>
               <property name="hibernate.cache.use_query_cache" value="true"/>
           </properties>
       </persistence-unit>
   </persistence>
   ```

   [^注意]: JPA 规范要求在类路径的 META-INF 目录下放置persistence.xml，文件的名称是固定的 

2. **创建实体类，使用注解来描述实体类跟数据库表之间的映射关系**

   ```java
   @Entity
   @Table(name = "class", schema = "test", catalog = "")
   public class ClazzEntity {
       private int id;
       private String name;
       private int age;
   
       @Id
       @Column(name = "id", nullable = false)
       public int getId() {
           return id;
       }
   
       public void setId(int id) {
           this.id = id;
       }
   
       @Basic
       @Column(name = "name", nullable = true, length = 10)
       public String getName() {
           return name;
       }
   
       public void setName(String name) {
           this.name = name;
       }
   
       @Basic
       @Column(name = "age", nullable = false)
       public int getAge() {
           return age;
       }
   
       public void setAge(int age) {
           this.age = age;
       }
   }
   ```

3. **使用 JPA API完成数据增加、删除、修改、和查询操作**





## JPA的基本注解

### @Entity

​	@Entity注解用于实体类声明之前，指定该Java类为实体类，将映射到指定的数据库表。如声明一个实体类Customer，它将映射到数据库中的customer表上。



### @Table

​	当实体类与其映射的数据库表名不同名时需要使用@Table标注说明，该注解与@Entity注解并列使用，置于实体类声明语句之前，可写于单独语句行，也可与声明语句同行。

[^name]: 指明数据库的表名
[^catalog和schema]: 设置表所属的数据库目录或模式，通常为数据库名。



### @Id

​	用于声明一个实体类的属性映射为数据库的主键列。该注释亦可置于属性的getter方法之前。



### @GeneratedValue

​	用于标注主键的生成策略，通过strategy属性指定。默认情况下，JPA自动选择一个最合适底层数据库的主键生成策略。SQLServer对应identity、MySql对应auto increment。

​	可选的主键生成策略有 :

- **identity**：采用数据库ID自增长的方式来自增主键字段，Oracle不支持这种方式。
- **auto**：JPA自动选择合适的策略，是默认的选择。
- **sequence**：通过序列产生主键，通过@SequenceGenerrator注解指定序列名，Mysql不支持这种方式。
- **table**：通过其他表产生主键，框架借由表模拟序列产生主键，使用该策略可以应用更易于数据库移植。



### 用table来生成主键

​	将当前主键的值单独保存到一个数据库的表中，主键的值每次都是从指定的表中查询后经过一定的算法后来获得。

​	这种方法生成主键的策略可以适用于任何数据库，不必担心不同数据库兼容造成的问题。

![用table来生成主键](/photo\用table来生成主键.bmp)

​	![用table来生成主键1](/photo\用table来生成主键1.bmp)

​	通过pkColumnName和pkColumnValue确定ID表中的一行，再通过valueColumnName确定主键列。随即每次实体表生成主键时都会查询该ID表中的主键列，再通过一定的算法后插入到实体表的主键列，而ID表中主键列也会自增长。



### @Column

​	该实体的属性与其映射的数据库的列不同名时需要使用该注解。该属性通常置于实体的属性声明语句之前，还可与@Id注解一起使用。

[^name]: 设置映射数据库表的列名
[^unique]: 设置该字段是否唯一
[^nullable]: 设置该字段是否允许为空
[^length]: 设置该字段的长度
[^columnDefinition]: 设置该字段在数据库中的实际类型，通常ORM框架可以根据属性类型自动判断数据库中字段的类型，但是对于Date类型仍无法确定数据库中字段类型究竟时DATE,TIME还是TIMESTAMP。此外，String的默认映射类型为VARCHAR，如果要将String类型映射到特定数据库的BLOB或TEXT字段类型，需要使用该注解确定数据库字段中的类型



### @Basic

	用于设置一个简单的属性到数据库表的字段的映射，对于没有任何标注的getXxx()方法，默认即为@Basic

[^fetch]: 设置该属性的读取策略，由eager和lazy两种，分别表示饥渴加载和懒加载，默认为eager。
[^optional]: 设置该属性是否允许为null，默认为true。



### @Transient

	表示该属性并非一个到数据库表的字段的映射，ORM框架将会忽略该属性。

​	

### @Temporal

​	在核心的	Java API中并没有定义Date类型的精度，而在数据库中，表示Date类型的数据由DATE、TIME、TIMESTAMP三种精度。在进行时间属性映射时可使用@Temporal注解来调整精度。



