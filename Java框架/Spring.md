# Spring概述

### 简介

​	Spring是于2003年兴起的一个轻量级的Java开发框架，它是为了解决企业应用开发的复杂性而创建的。Spring的核心是控制反转（IOC）和面向切面编程（AOP）。简单来说，Spring是一个分层的JavaSE/EE full-stack（一站式）轻量级开源项目。

​	Spring 的主要作用是为了代码“解耦”，降低代码间的耦合度。

​	根据功能的不同，可以将一个系统中的代码分为主业务逻辑两类。它们各自具有鲜明的特点：主业务代码间逻辑联系紧密，有具体的专业业务应用场景，复用性相对较低；系统级业务相对功能独立，没有具体的专业业务应用场景，主要是为主业务提供系统及服务，如日志，安全，事务等，复用性强。		

​	Spring根据代码的功能特点，将降低耦合度的方式分为两类，IOC和AOP。

​	IOC使得主业务在相互调用中，不用维护关系了，即不用再自己创建要使用得对象了。而是由Spring容器统一管理，自动“注入”。

​	而AOP使得系统级服务得到最大复用，且不用再由程序员手工将系统级服务“混杂”到主业务逻辑中了，而是由Spring容器统一完成“织入”。



### Spring体系结构

![Spring体系结构](E:\笔记\SSM\photo\Spring体系结构.bmp)

​	Spring由20多个模块组成，它们可以分为数据访问/继承（Data Access/Intergration）、Web、面向切面编程（AOP,Aspects）、应用服务器设备管理（Instrumentation）、信息发送（Messaging）、核心容器（Core Container）和测试（Test）。

### Spring的特点

> **非侵入式**

​	所谓非侵入式是指，Spring框架的API不会在业务逻辑出现，即业务逻辑是POJO。由于业务逻辑中没有Spring 的API，所以物业逻辑可以从Spring框架快速地移植到其他框架，即与环境无关。

​	POJO（Plain Old Java Object）:最存粹的类，无需其他引用项目的API。

> **容器**

​	Spring作为一个容器，可以管理对象的生命周期、对象与对象之间的依赖关系。可以通过配置文件，来定义对象，一以及设置与其他对象的依赖关系。

> IOC

​	控制反转（Inversion of Control），即创建被调者的实例不是由调用者完成，而是由Spring 容器完成，并注入给调用者。

​	当应用了IOC，一个对象依赖的其他对象通过被动的方式传递进来，而不是这个对象自己创建或者查找依赖对象。即，不是对象从容器中查找依赖，而是容器在对象初始化时不等对象请求就主动将依赖传递给它。

> AOP

​	面向切面编程（Aspect Orient Programming）,是一种编程思想，是面向对象编程OOP得补充。很多框架都实现了AOP编程思想得实现。Spring也提供了面向切面编程得丰富支持，允许通过分离应用得业务逻辑与系统级服务（例如日志和事务管理）进行开发。应用对象只实现它应该做得（完成业务逻辑）仅此而已。它们并不负责其他得系统级关注点，例如日志和事务支持。

​	我们可以把日志、安全、事务管理等服务理解成一个“切面”，那么以前这些服务一直是直接写在业务逻辑得代码当中。

​	这由两点不好，首先业务逻辑不纯净；其次这些服务被很多业务与逻辑反复使用，完全可以剥离出来做到服用。那么AOP就是解决这些问题的解决方案，可以把这些服务剥离出来形成一个“切面”，以期复用，然后将“切面”动态的“织入”到业务逻辑中，让业务逻辑能够享受到此“切面”的服务。

# IOC：控制反转

### 前言

​	控制反转是一个概念，是一种思想。即将传统上由程序代码直接操控的对象调用权交给容器，通过容器来实现对象的装配和管理。控制反转就是对对象控制权的转移，从程序代码本身转到了外部容器。

​	控制反转的实现由多种方式，现在主要流行的方式由两种：**依赖注入**和**依赖查找**，依赖注入方式应用更为广泛。也是Spring采用的方式。

- **依赖查找：**容器提供回调接口和上下文环境给组件，程序代码则需要提供具体的查找方式。比较典型的是依赖于JNDI服务接口的查找。

- **依赖注入：**程序代码不做定位查询，这些工作由容器自行完成。

  依赖注入DI是指程序运行过程中，若需要调用另一个对象协助时，无须在代码中创建被调用者，而是依赖于外部容器，由外部容器创建后传递给程序。

  **依赖注入目前时最优秀的解耦方式**。依赖注入让Sping和Bean之间以配置文件的方式在一起，而不是以硬解码的方式耦合在一起的。

### 创建测试项目

1. **使用IDEA创建Spring项目**

![创建Spring项目01](E:\笔记\Java框架\photo\创建Spring项目01.bmp)

![Spring项目结构](E:\笔记\Java框架\photo\Spring项目结构.bmp)

IDEA会自动下载Spring框架所需的Jar包



2. **创建Spring配置文件**

![创建Spring配置文件](E:\笔记\Java框架\photo\创建Spring配置文件.png)



3. **定义接口和实现类**

- 接口

```
public interface IServer {
    void print();
}

```

- 实现类

```
public class IServerImp implements IServer {
    @Override
    public void print() {
        System.out.println("HelloWorld");
    }

}

```



4. 定义测试类

```
public class Test01 {
    ApplicationContext context;
    
    @Before
    public void init() {
        context = new ClassPathXmlApplicationContext("applicationContext.xml");
    }

    @Test
    public void test() {
        IServer iServer = (IServer) context.getBean("server");
        iServer.print();
    }
}

```



5. 运行结果

![运行结果](E:\笔记\Java框架\photo\运行结果.bmp)



### 配置文件解析

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="server" class="com.jr.spring.test01.dao.imp.IServerImp"></bean>
</beans>
```

[^<bean/>]: 定义一个实例对象。一个实例对应一个bean元素
[^id]: 该属性是Bean实例的唯一标识，程序通过id属性访问Bean，Bean于Bean间的依赖关系也是通过id属性关联的。
[^name]: 该属性也是Bean实例的唯一标识
[^class]: 指定该Bean所属的类，注意这里只能是类，不能是接口

**id与name的区别**

​	一般情况下，命名\<bean/>使用id属性，而不使用name属性。在没有id属性的情况下，name属性与id属性的作用是相同的，当\<bean/>中含有一些特殊字符时，就需要使用那么属性了。

​	id的命名需要满足XML对id属性命名规范：必须以字母开头名可以包含字母、数字、下划线、连字符、句号、冒号。

​	name属性值则可以包含各种符号。

### 配置文件存放路径问题

​	根据获取容器实例（ApplicationContext）的对象不同，配置文件可以存放到随意路径。

> 存放到源代码根目录

![配置文件存放到源代码根目录](E:\笔记\Java框架\photo\Spring配置文件存放到源代码根目录.png)

若存放到源代码根目录，那么创建容器实例对象必须为**ClassPathXmlApplictionContext**

```
ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
```

[^ClassPathXmlApplicationContext(String configLocation)]: 配置文件在源代码根项目下的路径

**该对象会在源代码根目录下查找指定目录的配置文件资源**



> 存放到本地目录中

![Spring配置文件存放到本地磁盘目录中](E:\笔记\Java框架\photo\Spring配置文件存放到本地磁盘目录中.bmp)

若存放到项目根目录，那么创建容器实例对象必须为**FileSystemXmlApplictionContext**

```
ApplicationContext context = new FileSystemtXmlApplicationContext("applicationContext.xml");
```

[^FileSystemtXmlApplicationContext（String configPath）]: 项目根目录下的资源文件路径 或 磁盘空间下的文件路径

**该对象会从项目根目录下或磁盘空间下查找配置文件资源**



### BeanFactory接口容器

​	BeanFactory接口对象也可以作为Spring容器出现。BeanFactory接口时ApplicationContxt接口的父类。

​	其中XmlBeanFactory作为其中的实现类之一。

1. 创建XmlBeanFactory实例

```\
public class Test02 {
    BeanFactory beanFactory;

    @Before
    public void init() {
        beanFactory = new XmlBeanFactory(new ClassPathResource("applicationContext.xml"));
    }

    @Test
    public void test() {
        IServer iServer = (IServer) beanFactory.getBean("server");
        iServer.print();
    }
}
```

​	而Spring配置文件以资源Resouce的形式出现XmlBeanFactory类的构造器参数中。

​	Resouce时一个接口，其具有两个实现类：

- ClassPathResource：指定类路径下的资源文件
- FileSystemResource：指定项目根路径或本地磁盘路径下的资源文件



### 两个接口容器的区别

> **区别**

BeanFactory容器，对容器中对象的装配与加载采用延迟加载策略，即在第一次调用getBean()时，才真正装配该对象

```
而ApplicationContext容器，在容器初始化时就已经装配该对象。
```

**Bean的装配，即Bean对象的创建**

> **优势**

- **BeanFactory**：因为Bean对象是在调用者第一次调用getBean()方法时才装配Bean，所以BeanFactory相对不占用资源（内存和CUP），但响应速度慢。
- **ApplicationContext**：因为Bean对象在容器初始化时就已经装配了，所以使用ApplicationContext容器对资源的占用比较大，但响应速度快。

### Bean的装配

> 默认的装配方式

*代码举例test01*

**接口**

```
public interface IServer {
    void print();
}

```

**实现类**

```
public class IServerImp implements IServer {
    public IServerImp() {
        System.out.println("实例化IServerImp");
    }

    @Override
    public void print() {
        System.out.println("HelloWorld");
    }

}
```

**配置文件**

```
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
       
    <bean id="server" class="com.jr.spring.test01.dao.imp.IServerImp"></bean>
    
</beans>
```

**测试类**

```
public class Test01 {
    ApplicationContext context;

    @Before
    public void init() {
        context = new ClassPathXmlApplicationContext("com/jr/spring/test01/applicationContext.xml");
    }

    @Test
    public void test() {
        IServer iServer = (IServer) context.getBean("server");
        iServer.print();
    }
}
```

​	**代码通过getBean()方法从容器获取指定的bean实例，容器首先会调用Bean类的无参构造器，创造空值的实例对象。**



> Spring的工厂Bean

- **动态**

*代码举例test02*

​	Spring对于使用动态工厂来创建的Bean，有专门的属性定义。factory-bean指定相应的工厂Bean，由factory-method指定创建所用方法。此时配置文件中至少有两个Bean的指定：工厂类的Bean，与工厂类所要创建的目标Bean。而测试类中不需要获取工厂Bean对象了，可以直接获取目标Bean对象。实现测试类与工厂类间的解耦。

**工厂类**

```
public class ServerFactory {

    public IServer getServer() {
        return new IServerImp();
    }
}
```

**配置文件**

```
<beans xmlns="http://www.springframework.org/schema/beans"
               xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
               xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
               
<bean id="serverFactory" class="com.jr.spring.test02.dao.ServerFactory"></bean>
<bean id="server" factory-bean="serverFactory" factory-method="getServer"></bean>

</beans>
```

[^factory-bean]: 定义工厂Bean的id，声明生成Bean是哪个工厂对象
[^factory-method]: 工厂方法名



- **静态**

*代码举例test02*

​	使用工厂模式中的静态工厂来创建实例Bean。

​	此时需要注意，静态工厂无需工厂实例，所以不需要定义静态工厂\<bean/>。

​	而对于工厂所要创建的Bean，其不是由自己的类创建的，所以无需指定自己的类。但其是由工厂类创建的，所以需要指定所用工厂类。股class属性指定的是工厂类而非自己的类。当然，还需要通常factory-method属性指定工厂方法。

​	**工厂类**

```
public class ServerStaticFactory {

    public static IServer getServer() {
        return new IServerImp();
    }
}
```

​	**配置文件**

```
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--动态工厂-->
    <bean id="serverDynamicFactory" class="com.jr.spring.test02.dao.ServerDynamicFactory"></bean>
    <bean id="server01" factory-bean="serverDynamicFactory" factory-method="getServer"></bean>
    <!--静态工厂-->
    <bean id="server02" class="com.jr.spring.test02.dao.ServerStaticFactory" factory-method="getServer"></bean>

</beans>
```



> 容器中Bean的作用域

​	当通过Spring容器创建一个Bean实例时，不仅可以完成Bean的实例化，还可以通过**scope**属性，为Bean指定特定的作用域。Spring支持5种作用域。

1. **singleton**：单例模式。即每次使用getBean()方法获取的同一个\<bean/>的实例都是一个新的实例。
2. **prototype**：原型模式。即每次使用getBean()方法获取的同一个\<bean/>的实例都是一个新的实例。
3. **request**：对于每次的HTTP请求，都将会产生一个不同的Bean实例。
4. **session**：对于每个不同的HttpSession，都将会产生一个不同的Bean实例。

**注意：**

​	1）.对于scope的值request，session与globle session，只有Web应用中使用Spring时，该作用域才有效。

​	2）.对于scope为singleton的单例模式，该Bean是在容器被创建时即被装配好了。

​	3）.对于scope为protoype的原型模式，Bean实例时在代码使用Bean实例时才进行装配的。



> Bean后处理器

​	Bean后处理器是一种特殊的Bean，容器中所有的Bean在初始化时，均会自动执行该类的两个方法。由于该Bean是由其他Bean自动调用执行，而不是程序员手工调用，故此Bean无需id属性或name属性。

​	需要做的是，在Bean后处理器类方法中，只要对Bean类与Bean类中的方法进行判断就可实现指定的Bean的指定方法进行功能扩展和增强。方法返回的Bean对象，即是增强过的对象。

*举例test03*

​	程序中由一个业务接口，其有两个业务方法，some（）与other（）。有两个Bean，StudentServicelmpl()和TeacherServicelmpl()，均实现了IService接口。

​	要求：对StudentServiceImpl的some方法进行增强，输出其开始执行时间与执行结束时间。



**接口**

```
public interface IServer {
    void some();

    void other();
}
```

**实现类**

```
public class StudentServicelmp implements IServer {
    @Override
    public void some() {
        System.out.println("StudentServicelmp:some()");
    }

    @Override
    public void other() {
        System.out.println("StudentServicelmp:other()");
    }
}
```

**Bean后处理器类**

```
public class MyBeanPostProcessor implements BeanPostProcessor {
    @Override
    public Object postProcessBeforeInitialization(Object bean, String id) throws BeansException {
        if (bean instanceof StudentServicelmp) {
            //通过动态代理增强Bean方法
            IServer studentService = (IServer) Proxy.newProxyInstance(bean.getClass().getClassLoader(), bean.getClass().getInterfaces(), new InvocationHandler() {
                @Override
                public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                    if (method.getName().equals("some")) {
                        System.out.println("Bean后处理器通过动态代理增强的方法:当前执行方法前的时间" + System.currentTimeMillis());
                        Object object = method.invoke(bean, args);
                        System.out.println("Bean后处理器通过动态代理增强的方法:当前执行方法后的时间" + System.currentTimeMillis());
                        return object;
                    }
                    return method.invoke(bean, args);
                }
            });
            return studentService;
        }
        return null;
    }

    @Override
    public Object postProcessAfterInitialization(Object bean, String id) throws BeansException {
        return bean;
    }
}
```

​	代码中需要定义Bean后处理器类。该类就是实现了BeanPostProcessor的类。该接口中包含两个方法，分别在目标Bean**初始化之前和之后**执行，它们的返回值为功能被扩展后的Bean对象。

[^postProcessBeforeInitialization(Object bean, String id)]: 该方法会在目标Bean初始化完毕之前由容器自动调用。
[^bean]: 系统即将初始化的Bean的实例
[^id]: 定义Bean的id，若没有id就是name 的属性值。
[^postProcessAfterInitialization(Object bean, String id)]: 该方法会在目标Bean初始化完毕之后用容器自动调用。注意！！！即使不对Bean进行增强，也要在该方法返回bean，不能默认为null，否则会抛出NullPointerException。

**配置文件**

```
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--定义Bean-->
    <bean id="StudentServer" class="com.jr.spring.test03.dao.imp.StudentServicelmp"></bean>
    <bean id="TeacherServer" class="com.jr.spring.test03.dao.imp.TeacherServicelmp"></bean>
    <!--定义Bean后处理器-->
    <bean class="com.jr.spring.test03.dao.MyBeanPostProcessor"></bean>
</beans>
```

​	Bean初始化完毕有一个标志：在生命周期中的一个方法会被执行（下面会讲到Bean的生命周期），即当该方法被执行，表示该bean被初始化完毕。

​	所以Bean后处理器中两个方法的执行，是在这个方法之前和之后执行。



> 定制Bean的生命始末

​	可以为Bean定制初始化后的生命行为，也可以为Bean定制销毁前的生命行为

*举例test04*

**实现类**

```
public class StudentServicelmp implements IServer {
    @Override
    public void some() {
        System.out.println("StudentServicelmp:some()");
    }

    @Override
    public String other() {
        System.out.println("StudentServicelmp:other()");
        return null;
    }

    public void init() {
        System.out.println("init-method");
    }

    public void destory() {
        System.out.println("destory-method");
    }
}
```

**配置文件**

```
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--定义Bean-->
    <bean id="StudentServer" class="com.jr.spring.test04.dao.imp.StudentServicelmp" init-method="init"
          destroy-method="destory"></bean>
</beans>
```

[^init-method]: 声明bean初始化后调用的方法
[^destroy-method]: 声明bean销毁前调用的方法

若要看到Bean的destory-method的执行。需要满足两个条件

1. Bean为singleton，即单例
2. 要确保容器关闭，接口ApplicationContext没有close()方法，但其实现类有，所以，可以将ApplicationContext强转为其实现类对象，或者直接创建就是实现类对象



> Bean的生命周期

​	Bean实例从创建到最后销毁，需要经常很多过程，执行很多生命周期方法。

*举例test05*

1. 调用无参构造器，创建实例对象。
2. 调用参数的setter，为属性注入值。
3. 若Bean实现了BeanNameAware接口，则会执行接口方法setBeanName(String beanId)，使Bean类可以获取在其在容器的id名称。
4. 若Bean实现了BeanFactoryAware接口，则执行接口方法的setBeanFactory（BeanFactory factory），使Bean类可以获取BeanFactory对象。
5. 若定义并注册了Bean后处理器BeanPostProcessor，则执行接口的postProcessBeforInitialzation()。
6. 若Bean实现了InitalizingBean接口，则执行接口方法的afterPropertiesSet()。该方法在Bean的**所有属性的set方法执行完毕后执行，使Bean初始化结束的标志，即Bean实例化结束**。
7. 若设置了init-method方法，则执行其中定义的方法。
8. 若定义并注册了Bean后处理器BeanPostProcessor，则执行接口方法的postProcessAfterInitialzation()。
9. 执行业务方法。
10. 若Bean实现了DisposableBean接口，则执行接口方法destory()。
11. 若设置了destory-method方法。则执行其中定义的方法。



# 依赖注入

### 基于XML的DL





### 基于注解的DL

