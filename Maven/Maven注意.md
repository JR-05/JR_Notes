### Maven工程注意

- **Maven 工程 中， .xml 或 .properties 配置文件无法找到** 

在Maven工程中，xml、properties等资源不会自动被编译，需要在配置文件中指定编译资源

```
<bulid>
        <resources>
            <!-- 编译之后包含xml和properties -->
            <resource>
                <directory>src/main/java/resources</directory>
                <includes>
                    <include>**/*</include><!--**:表示所有上级文件文件夹;*:表示所有文件-->
                    <include>*.xml</include><!--表示编译所有xml文件F-->
                </includes>
                <filtering>true</filtering>
            </resource>
        </resources>
</bulid>
```



- **将资源统一存放问题**

将xml，properties等配置文件统一放到resoures文件目录

![资源存放目录](E:/%E7%AC%94%E8%AE%B0/Java%E6%A1%86%E6%9E%B6/photo/%E8%B5%84%E6%BA%90%E5%AD%98%E6%94%BE%E7%9B%AE%E5%BD%95.bmp)

注意引用文件的路径是编译后的文件路径

![资源路径](E:/%E7%AC%94%E8%AE%B0/Java%E6%A1%86%E6%9E%B6/photo/%E8%B5%84%E6%BA%90%E8%B7%AF%E5%BE%84.bmp)



