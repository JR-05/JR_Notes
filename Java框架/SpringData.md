# SpringData前言

###介绍

	Spring Data : Spring 的一个子项目。用于简化数据库访问，支持NoSQL 和 关系数据存储。其主要目标是使数据库的访问变得方便快捷。

- SpringData 项目所支持 NoSQL 存储：

1. MongoDB （文档数据库）

2. Neo4j（图形数据库）

3. Redis（键/值存储）

4. Hbase（列族数据库）

-  SpringData 项目所支持的关系数据存储技术：

1. JDBC

2. JPA

3. ject

**本笔记学习的是SpringData与JPA结合的知识**

### 作用

	JPA Spring Data : 致力于减少数据访问层 (DAO) 的开发量. 开发者唯一要做的，就只是声明持久层的接口，其他都交给 Spring Data JPA 来帮你完成！

	框架怎么可能代替开发者实现业务逻辑呢？比如：当有一个 UserDao.findUserById()  这样一个方法声明，大致应该能判断出这是根据给定条件的 ID 查询出满足条件的 User  对象。Spring Data JPA 做的便是规范方法的名字，根据符合规范的名字来确定方法需要实现什么样的逻辑。



### 环境搭建

- 创建Maven工程并除了基本的**Spring骨架外与JPA规范的包外**还需依赖两个SpringData与JPA的Jar包

  ```xml
          <!--SpringData基础包-->
          <dependency>
              <groupId>org.springframework.data</groupId>
              <artifactId>spring-data-commons</artifactId>
              <version>1.6.2.RELEASE</version>
          </dependency>
          <!--JPA与SpringData兼容包-->
          <dependency>
              <groupId>org.springframework.data</groupId>
              <artifactId>spring-data-jpa</artifactId>
              <version>1.4.2.RELEASE</version>
          </dependency>
  ```

  **注意SpringData与Spring骨架的其它包版本需一致**




# Repository接口

###概述

​	Repository 接口是SpringData 的一个核心接口，它不提供任何方法，开发者需要在自己定义的接口中声明需要的方法。

​	SpringData的主要亮点就是开发者定义操作数据库Dao层接口时，无需再写出实现类，而是根据SpringData给出的接口方法名策略，根据它命名的要求，再按照开发者的需求写出接口方法即可。如：

​	`UserDao.findUserById()`

​	这样一个方法声明，大致应该能判断出这是根据给定条件的ID查询出满足条件的User对象。SpringData JPA做的便是规范方法的名字，根据符合规范的名字来确定方法需要实现什么样的逻辑。	

### 声明SpringData的Dao接口

​	与继承 Repository 等价的一种方式，就是在持久层接口上使用 @RepositoryDefinition 注解，并为其指定 domainClass 和 idClass 属性。如下两种方式是完全等价的

- **继承Repository接口**

  ```java
  public interface StudentRepositoryDao extends Repository<Student, Integer> {
  ```

  其中Repository两个泛型分别为`映射实体对象`，`实体对象中的主键类型`。

- **@RepositoryDefinition**

  ```java
  @RepositoryDefinition(domainClass = Teacher.class, idClass = Integer.class)
  public class TeacherRepositoryDefinition {
  ```

  [^domainClass]: 映射实体对象
  [^idClass]: 实体对象中的主键类型

### Repository的继承关系

​	基础的 Repository提供了最基本的数据访问功能，其几个子接口则扩展了一些功能。它们的继承关系如下：

#### Repository

	仅仅是一个标识，表明任何继承它的均为仓库接口类。

#### CrudRepository

​	 继承 Repository，实现了一组 CRUD 相关的方法 

#### PagingAndSortingRepository

​	继承 CrudRepository，实现了一组分页排序相关的方法 

#### JpaRepository

​	继承 PagingAndSortingRepository，实现一组 JPA 规范相关的方法 

#### 自定义的 XxxxRepository

​	需要继承 JpaRepository，这样的 XxxxRepository 接口就具备了通用的数据访问控制层的能力。

#### JpaSpecificationExecutor

​	不属于Repository体系，实现一组 JPA Criteria 查询相关的方法 



# SpringData 方法定义规范

### 简单条件查询

**规范**

​	按照Spring Data 的规范，查询方法以`find` | `read` | `get` 开头， 涉及条件查询时，条件的属性用条件关键字连接，要注意的是：条件属性以首字母大写。 

**举例：**

定义一个 Entity实体类 

```java
classUser｛ 
  private StringfirstName; 
  private StringlastName; 
｝ 
```

使用And条件连接时，应这样写` findByLastNameAndFirstName(StringlastName,StringfirstName);`

条件的属性名称与个数要与参数的位置与个数一一对应。



###支持的关键字
|       关键字       |                举例                 |          最终转换成的JQPL语句片段           | 解释                                                         |
| :----------------: | :---------------------------------: | :-----------------------------------------: | ------------------------------------------------------------ |
|        And         |     findByLastnameAndFirstname      |   ...where x.lastname=?1 and firstname=?2   | 和                                                           |
|         Or         |      findByLastnameOrFirstname      |  ...where x.lastname=?1 or x.firstname=?2   | 或                                                           |
|      Between       |       findByStartDateBetween        |   ...where x.startDate between ?1 and ?2    | 之间                                                         |
|      LessThan      |          findByAgeLessThan          |              ...where x.age<?1              | 小于                                                         |
|    GreaterThan     |        findByAgeGreaterThan         |              ...where x.age>?1              | 大于                                                         |
|       After        |        findByStartDateAfter         |           ...where x.startDate>?1           | 之后，判断时间类型                                           |
|       Before       |        findByStartDateBefor         |           ...where x.startDate<?1           | 之前，判断时间类型                                           |
|       lsNull       |           findByAgeIsNull           |           ...where x.age is null            | 等于null                                                     |
| IsNotNull==NotNull |        findByAge(Is)NotNull         |           ...where x.age not null           | 不等于null                                                   |
|        Like        |         findByFirstnameLike         |        ...where x.firstname like ?1         | 根据传入参数进行匹配筛选**类似**字段数据                     |
|      NotLike       |       findByFirstnameNotLike        |      ...where x.firstname not like ?1       | 根据传入参数进行匹配筛选**不类似**字段数据                   |
|    StartingWith    |     findByFirstnameStartingWith     |      ...where x.firstname not like ?1       | 匹配字段**开头**，底层转换JQPL语句实际在参数**前面**添加了'%'占位符 |
|     EndingWith     |      findByFirstnameEndingWith      |      ...where x.firstname not like ?1       | 匹配字段**末尾**，底层转换JQPL语句实际在参数**后面**追加了'%'占位符 |
|     Containing     |      findByFirstnameContaining      |      ...where x.firstname not like ?1       | 匹配字段**包含**参数字符串，底层转换JQPL语句实际在参数**首尾**都添加了‘%’占位符 |
|      OrderBy       |    findByFirstnameOrerByAgeDesc     | ...where x.firstname=?1 order by x.age desc | 指定字段进行排序                                             |
|        Not         |          findByLastnameNot          |          ...where x.lastname <>?1           |                                                              |
|         In         |  findByAgeIn(Collection<Age> age)   |            ...where x .age in ?1            | 筛选出指定字段**在**该参数集合条件下的数据列                 |
|       NotIn        | findByAgeNotIn(Collection<Age> age) |          ...where x.age not in ?1           | 筛选出指定字段**不在**该参数集合条件下的数据列               |
|        True        |         findByActiveTrue()          |           ...where x.active=true            |                                                              |
|       False        |         findByActiveFalse()         |           ...where x.active=false           |                                                              |



# HelloSpringData

​	使用 SpringData JPA 进行持久层开发需要的三个步骤：

1. **在 Spring的配置文件中配置SpringData**

   ```xml
   <jpa:repositories base-package="dao"
                     entity-manager-factory-ref="entityManagerFactory"
                     transaction-manager-ref="transactionManager"/>
   ```

   [^base-package]: Dao层包名
   [^entity-manager-factory-ref]: EntityManagerFactory引用
   [^ransaction-manager-ref]: 事务管理器的引用

2. **声明持久层的接口，该接口继承  Repository**

   ​	Repository 是一个标记型接口（类似标识对象序列化的Serializable），它不包含任何方法，如必要，SpringData 可实现 Repository其他子接口，其中定义了一些常用的增删改查，以及分页相关的方法。

   ```java
   public interface StudentRepositoryDao extends Repository<Student, Integer> {
   ```

   ​	



3. **在接口中声明需要的方法**

   ​	Spring Data 将根据给定的策略（具体策略稍后讲解）来为其生成实现代码。

   ```java
   public interface StudentRepositoryDao extends Repository<Student, Integer> {
       Student findById(Integer id);
   }
   ```


