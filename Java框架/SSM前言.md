# 有关包的命名

### 意义

将类定义的进行分包，在一定意义上使得该类具有全球的唯一性，不与其他类进行冲突

### 分包

包可分为四到五层

- 第一层：甲方公司域名的倒序。如：com.jr
- 第二层：项目名称。
- 第三层：项目模块信息
- 第四层：功能顶层包。如：serivce，dao，beans
- 第五层：实现类层。如：imp。有些功能顶层包是没有实现类层的，所以有些项目包名知到四层

### 数据封装层的分类

1. 根据数据库表字段映射

- beans
- entity
- po（Persistance Object）持久化对象

上面这些包中存放的类，一般具有id属性。因为它们在数据库中有对应的表

2. 根据实际业务需求数据映射

- vo（Value Object）值对象。一般用于在类与页面间传值
- dto（Data Transter Object）数据传输对象。一般用于在类间传值

上面这些包中存放的类，一般是没有id属性的。因为他们不需要持久化到数据库







# XML选择本地DTD不走网络

![设置XML不提示问题](E:\笔记\SSM\photo\设置XML不提示问题.bmp)





# Log4j

### 配置文件详解

- log4j.rootLogger=[level]，appenderName1，appenderName2......

level：是日志记录的优先级，分为OFF，FATAL，ERROR，WARN，DEBUG

通过在这里定义的级别，你可以控制到应用程序中相应级别的日志信息的开关。比如定义了INFO级别，则应用程序中所有DEBUG级别的日志信息将不被打印出来。

appenderName：就是制定日志信息输出到哪个地方。你可以同时指定多个输出目的地。例如：log4j.rootLogger=info,**A**,B,C配置了三个输出地方，这个名字可以随意（如上面的A），但必须与我们在后面进行的设置名字对应



- log4j.appender.**A**=[输出类型]

定义名为A的输出端是哪种输出类型，可以是

[^org.apache.log4j.ConsoleAppender]: 输出到控制台
[^org.apache.log4j.FileAppender]: 输出到文件
[^org.apache.log4j.DailyRollingFileAppender]: 每天产生一个日志文件
[^org.apache.log4j.WriterAppender]: 将日志信息以流格式发送到指定的地方



- log4j.appender.A.layout=[布局格式]

定义名为A的输出端的layout是哪种类型，可以是

[^org.apache.log4j.HTMLLayout]: 以HTML表格形式布局
[^org.apache.log4j.SimpleLayout]: 包含日志信息的级别和信息字符串
[^org.apache.log4j.TTCCLayout]: 包含日志产生的时间，线程，类别等信息
[^org.apache.log4j.PatternLayout]: 可以灵活指定布局模式，后面需要在配置ConversionPattern



- log4j.appender.A.layout,ConversionPattern=自定义模板

打印参数如下：

[^%m]: 输出代码中指定的信息
[^%M]: 输出打印该条日志的方法名
[^%p]: 输出优先级，即DEBUG，INFO，WARN，ERROR，FATAL
[^%r]: 输出自应用启动到输出该log信息所耗费的毫秒数
[^%c]: 输出所属的类目，通常就是所在类的全名
[^%t]: 输出产生该日志事件的线程名
[^%d]: 输出日志时间点的日期或时间，默认格式为ISO8601,也可以在其后指定格式，比如：%d{yyyy-MM-dd HH:mm:ss,SSS},输出：2018-05-26 15：00：50，666
[^%l]: 输出日志事件的发生位置，即代码中的行数
[^%n]: 输出一个回车换行符



- log4j.appender.A.File=path

指定文件输出路径



- log4j.appender.A.Append=true|false

 是否追加输入文件

[^ture]: 那么每次产生的新的log信息都会追加到原来的数据
[^false]: 新的log信息输入到文件之前会清空原来的log数据后在输入到文件



- log4j.appender.A.Threshold=[level]

指定到该输出端的log日志级别，假如为ERROR,那么当出现ERROR级别的日志就会被该定义的输出端输出到指定位置

