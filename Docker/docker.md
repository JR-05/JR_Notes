##  介绍

	Docker是一种类似于与虚拟机技术，是一种虚拟容器技术。
	是一个容器运行载体或称之为容器管理引擎。我们把应用程序和配置依赖打包好形成一个可交付的运行环境，这个打包好的运行环境就似乎image镜像文件。
	但与虚拟机技术不同的是，在Docker容器上运行的程序并不是一个完整的操作系统+软件，Docker舍弃了传统虚拟机容器中需要的Hypervisor硬件虚拟化，运行在Docker容器上的程序直接使用实际物理机的硬件资源。因此在CPU，内存利用率上Docker将会在效率上有明显优势。并且Docker的镜像文件也比平常的镜像文件小很多。

### Docker三要素

	Docker的组成架构

#### 仓库

	是集中存放镜像文件的场所 

#### 镜像

	它可以看作是一个轻量级的Linux操作系统+应用程序，但它省去了硬件Hypervisor虚拟化，而又它只能运行在Docker引擎内。

我们把应用程序和配置依赖打包好形成一个可交付的运行环境，这个打包好的运行环境就似乎image镜像文件。



#### 容器

  	我们通过这个镜像文件生成的实例称为容器，image镜像文件可以看作是容器的模板。同一个image文件可以生成多个同时运行的容器实例。
就好比class类可以生成多个实例对象。



##几个Linux命令

1. [^ifconfig]: 查看网络信息

2. [^ps]: 所有所有进程

3. [^top]: 当前所有完整的进程信息

4. [^ls]: 列出当前所有子目录

5. [^vi]: 打开文本编辑器

6. [^vim]: vi的升级版

7. [^cat]: 查看文件里面的内容

8. [^touch]: 创建文件

9. [^mkdir]: 创建目录

10. [^/bin/bash]: 启动命令终端




## Docker的安装

	在Centos7环境下安装Docker

	Docker从1.13版本之后采用时间线的方式作为版本号，分为社区版CE和企业版EE。

	社区版是免费提供给个人开发者和小型团体使用的，企业版会提供额外的收费服务，比如经过官方测试认证过的基础设施、容器、插件等。



1. **Docker 要求 CentOS 系统的内核版本高于 3.10 ，查看本页面的前提条件来验证你的CentOS 版本是否支持 Docker 。**

```shell
$ sudo uname -r	
```

[^$sudo]: 表示管理员权限运行命令

2. **使用 `root` 权限登录 Centos。确保 yum 包更新到最**

```shell
$ sudo yum update
```

[^yum]: 全称"yellow dog update modify"，可以看作是一个应用市场，用于应用程序的更新和下载

3. **卸载旧版本(如果安装过旧版本的话)**

```shell
$ sudo yum remove docker  docker-common docker-selinux docker-engine
```

4. **安装需要的软件包， yum-util 提供yum-config-manager功能，另外两个是devicemapper驱动依赖的**

```shell
$ sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```

	对于首次安装Docker的用户，需要下载Docker仓库，之后你才能从Docker仓库上更新或下载Docker。

5. **设置Stable镜像仓库**

```shell
$ sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

	执行完毕后，就会产生一个/etc/yum.repos.d/docker-ce.repo文件，会自动设置镜像仓库链接地址，也就是之后的镜像下载更新地址。
	
	通常一个repo文件定义了一个或者多个软件仓库的[细节](http://gcell.yo2.cn/archives/tag/%E7%BB%86%E8%8A%82)内容，例如我们将从哪里下载需要[安装](http://gcell.yo2.cn/archives/tag/%E5%AE%89%E8%A3%85)或者升级的软件包，repo文件中的设置内容将被yum读取和应用！

6. **可以查看所有仓库中所有docker版本，并选择特定版本安**

```shell
$ yum list docker-ce --showduplicates | sort -r
```

7. **安装docker**

```shell
$ sudo yum install docker-ce  #由于repo中默认只开启stable仓库，故这里安装的是最新稳定版17.12.0
$ sudo yum install <FQPN>  # 例如：sudo yum install docker-ce-17.12.0.ce
```

8. **启动并加入开机启动**

```shell
$ sudo systemctl start docker #启动
$ sudo systemctl enable docker #开机启动
```

9. **验证安装是否成功(有client和service两部分表示docker安装启动都成功了)**

```shell
$ docker version
```

10. **配置镜像加速器**

```shell
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://cdbvzi1l.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

	解决国内网络延迟


	



## Docker命令

**注意，Docker命令必须在管理员权限下执行**

### docker help

	查看所有Docker命令及解释

>语法

```
docker 指定命令 --help #查询指定帮助命令
```

![docker_xxx--help](/photo\docker_xxx--help.png)

```
docker --help  #查询帮助所有Docker命令
```

![docker_help](/photo\docker_help.png)



### docker info

	查看当前Docker信息，包括所有镜像文件，当前正在运行容器，关闭容器，以及Docker引擎运行环境...

> 语法

```shell
docker info
```

![docker_info](/photo\docker_info.png)

### docker search 

	查看仓库服务器内的所有该镜像信息

> 语法	

```shell
docker search [OPTIONS]
```

![docker_search](/photo/docker_search.png)

> 标记

1. [^START]: 点赞量

2. [^OFFICIAL]: 是否为官方版本

3. [^AUTOMATED]: 是否为自动构建版本

> OPTIONS（可加参数）

1. --no-trunc：显示完整的镜像信息

   ```shell
   docker search --no-trunc 镜像名
   ```

2. -s：列出收藏数不小于指定值的镜像

   ```shell
   docker search -s 500 镜像名
   ```

3. -automated：只列出automated build类型的镜像

   ```shell
   docker search -automated 镜像名
   ```

4. 可追加参数

   ```shell
   docker search -s -automated 镜像名
   ```



### docker login|logout

	登录或退出到一个Docker镜像仓库，如果未指定镜像仓库地址，默认为官方的Docker Hub

> 语法

```shell
docker login [OPTIONS] [SERVER]
```

> OPTIONS（可加参数）

1. [^-u]: 登录的用户名

2. [^-p]: 登录的密码

> 案例

```shell
docker login -u -p registry.cn-jr.aliyuncs.com
```

备注：登录阿里云docker远程仓库



### docker tag

	标记本地镜像，将其归入某一仓库

>语法

```shell
docker tag 镜像ID 仓库名[:版本号]
```

> 案例

```shell
docker tag xxx registry.cn-jr.aliyuncs.com/仓库名：版本号
```





###docker pull

	下载镜像

> 语法

- **不指定TAG（默认下载latest最新版本）**

  ```shell
  docker pull mysql
  ```

- **指定TAG（指定镜像版本）**

  ```
  docker pull mysql:TAG
  ```



### docker push

	将本地的镜像上传到镜像仓库，前提是先登录镜像仓库，并将上传的镜像打上tag

> 语法

```shell
docker push [OPTIONS] 仓库名[:版本] #可不加版本，默认为latest
```

> OPTIONS（可加参数）

1. [^disable-content-trust]: 忽略镜像的校验，默认开启






### docker images

	查看本地仓库下所有的镜像文件

> 语法

```shell
docker images [OPTIONS]
```

![docker_images](/photo\docker_images.png)

> 标记

1. [^TAG]: 镜像标签，比如同一个Mysql镜像有多个版本，那么TAG就是用于区分不同版本的

2. [^REPOSITORY]: 镜像仓库源，可用于操作镜像命令用的

3. [^SIZE]: 镜像大小

4. [^IMAGE ID]: 区分本地不同镜像ID，可用于操作镜像命令用的**CREATED**：镜像在本地创建时间

> OPTIONS（可加参数）

1.  -a :列出本地所有镜像

   ```shell
   docker images -a
   ```

2. -q :只显示镜像ID

   ```shell
   docker images  -q
   ```

3.  --digests：显示镜像的摘要信息

   ```shell
   docker images --digests
   ```

4.  --no-trunc：显示完整的镜像信息

   ```shell
   docker images --no-trunc
   ```

5. **可追加参数，如：**

   [^docker images -aq]: 显示所有镜像的ID

   ![docker_images -aq](/photo\docker_images -aq.png)

   [^docker imgaes -a --no-trunc]: 显示所有镜像的完整信息

   ![docker_images -a --no-trunc](/photo\docker_images -a --no-trunc.png)

### docker rmi 

	删除镜像

> 语法

- 删除单个镜像

  ```shell
  docker  rmi 镜像ID
  ```

- 删除多个镜像

  ```shell
  docker rmi 镜像名1：TAG 镜像名2：TAG 
  ```

- 动态删除多个容器

  ```shell
  docker rmi $(docker images -aq)
  ```

  [^$(dokcer images -aq)]: 返回docker images -aq 查询出来的所有镜像ID

> 可加参数

- -f

  ```shell
  docker rmi -f 镜像ID
  ```

  强制删除，即使存在正在运行的该镜像容器

### docker run

	新建并启动一个容器

> 语法

```shell
docker run [OPTIONS] 镜像ID [COMMAND] [ARG...]
```

> OPTIONS（可加参数）

[^--name]: 指定容器名，在之后容器操作中使用
[^--link]: 添加链接到另一个容器
[^--expose=[\]]: 开放一个端口或一组端口

	我们在使用Docker的时候，经常可能需要连接到其他的容器，比如：web服务需要连接数据库。按照往常的做法，需要先启动数据库的容器，映射出端口来，然后配置好客户端的容器，再去访问。其实针对这种场景，Docker提供了–link 参数来满足。

[^-d]: 后台式启动容器（也称守护式容器），成功返回容器ID
[^-i]: 前台式启动容器（也称交互式容器），通常与-t同时使用
[^-t]: 为容器重新分配一个伪输入终端，通常与-i同时使用
[^-P]: 随机端口映射
[^-p]: 指定端口映射，有以下四种映射格式

1. ip:主机（宿主）端口:容器端口
2. ip::容器端口
3. 主机（宿主）端口:容器端口
4. 容器端口

[^-m]: 设置容器使用内存最大值
[^-v]: 设置数据卷映射路径
[^-e]: 设置环境变量，比如在启动mysql配置用户名和密码时需要使用到该参数



> COMMAND

```
docker run -it centos /bin/bash
```

启动后在容器内执行/bin/bash

> 注意

1. Docker容器后台运行，必须有一个前台进程，否则就会自动退出



### docker create

	创建一个新的容器但不启动它

> 语法

```shell
docker create [OPTIONS] 镜像ID [执行命令] [启动参数]
```

> OPTIONS（可加参数）

	如同docker run



### docker ps

	查看docker创建的容器，类似于linux中的ps。不加-a，默认列出当前正在运行的容器进程信息。

> 语法

```
docker ps [OPTIONS]
```

> OPTIONS（可加参数）

[^-a]: 显示所有的容器，包括未运行的
[^-f]: 根据条件过滤显示的内容

```shell
 docker ps -f before=镜像ID #before过滤在容器9c3527ed70ce之前创建的容器
```

```shell
 docker ps -f since=镜像ID #since过滤器显示在给定容器id或名称之后创建的容器
```

[^-l]: 显示最近创建的容器
[^-n]: 显示最近创建的n个容器

```shell
docker ps -n 5
```

[^-q]: 只显示容器ID
[^-s]: 只显示总的文件大小
[^--no-trunc]: 不截断输出



### docker start

	启动已经被停止的容器

> 语法

```shell
docker start [OPTIONS] 容器ID [启动参数...]
```



### docker restart

	重新启动容器

> 语法

```shell
docker restart [OPTIONS] 容器ID [启动参数...]
```



### docker stop

	停止一个正在运行的容器，正常关闭

> 语法

```shell
docker stop [OPTIONS] 容器ID [启动参数...]
```



### docker kill

	直接杀死容器进程，相当于直接拔掉电源

> 语法

```shell
docker kill [OPTIONS] CONTAINER [启动参数...]
```



### docker pause

	暂停容器中的所有进程

> 语法

- 暂停单个容器

  ```shell
  docker pause [OPTIONS] 容器ID 
  ```

- 暂停多个容器

  ```shell
  docker pause [OPTIONS] 容器ID...
  ```

- 动态暂停多个容器

  ```shell
  docker pause $(docker ps -aq)
  ```



### docker unpause

	恢复容器中的所有进程

> 语法

- 恢复单个容器

  ```shell
  docker pause [OPTIONS] 容器ID 
  ```

- 恢复多个容器

  ```shell
  docker pause [OPTIONS] 容器ID...
  ```

- 动态恢复多个容器

  ```shell
  docker unpause $(docker ps -aq)
  ```



### 退出当前交互式容器

- exit

  退出并关闭容器

- ctrl+P+Q

  退出并保持容器以后台进行



### docker rm

	删除一个或多个容器

> 语法

- 删除单个容器

  ```shell
  docker rm [OPTIONS] 容器ID 
  ```

- 删除多个容器

  ```shell
  docker rm [OPTIONS] 容器ID...
  ```

- 动态删除多个容器

  ```shell
  docker rm $(docker ps -aq)
  ```



> OPTIONS（可加参数）

1. [^-l]: 移除容器间的网络连接，而非容器本身

2. [^-v]: 删除与容器相关联的数据卷



### docker logs

	获取容器的日志，也就是容器的echo输出

> 语法

```shell
docker logs [OPTIONS] 容器ID
```



> OPTIONS（可加参数）

[^-f]: 跟踪日志输出
[^-t]: 显示时间戳
[^--since]: 显示某个时间开始的所有日志
[^--tail]: 仅列出最新的N条容器日志

```
docker logs --since="2016-07-01" --tail=10 mynginx
```

备注：查看容器mynginx从2016年7月1日后的最新10条日志



### docker top

	查看容器内正在运行的进程，类似于Linux的top

> 语法

```shell
docker top 容器ID
```



### docker inspect

	查看容器内部细节，包括容器ID、创建时间、运行命令、运行参数、容器数据卷映射...

> 语法

```shell
docker inspect [OPTIONS] 容器ID 
```

> OPTIONS（可加参数）

1. [^-f]: 指定返回值得模板文件

2. [^-s]: 显示总的文件大小

3. [^--type]: 指定返回类型为JSON



### docker attach

	进入正在运行的容器

> 语法

```shell
docker attach [OPTIONS] 容器ID
```



### docker exec

	进入正在运行的容器后执行参数命令

> 语法

```shell
docker exec [OPTIONS] 容器ID 执行命令 [ARG...]
```

- 进入centos容器并查看当前目录结构，返回结果退出

  ```shell
  docker exec centos ls -l	
  ```

- 进入centos容器启动命令终端，不退出

  ```shell
  docker exec centos /bin/bash
  ```

> OPTIONS（可加参数）

1. [^-d]: 后台运行

2. [^-i]: 即使没有附加也保持STDIN打开

3. [^-t]: 分配一个伪终端


### docker cp

	用于容器与主机之间的数据拷贝

> 语法

- 主机拷向容器

  ```shell
  docker cp [OPTIONS] 主机保存路径 容器ID：文件路径 
  ```


- 容器拷向主机

  ```shell
  docker cp [OPTIONS] 容器ID：文件路径 主机保存路径
  ```


### docker commit

	以容器为模板创建一个新的镜像

> 语法

```shell
docker commit [OPTIONS] 容器ID [REPOSITORY[:TAG]]
```

> OPTIONS（可加参数）

1. [^-a]: 作者

2. [^-c]: 使用Dockerfile指令来创建镜像

3. [^-m]: 说明文字

4. [^-p]: 在commit时，将容器暂停

> 实例

```shell
docker commit -a "runoob.com" -m "my apache" a404c6c174a2  mymysql:v1
```

	将容器a404c6c174a2 保存为新的镜像,并添加提交人信息和说明信息。若不指定镜像名:TAG，默认根据容器来创建。



### docker build

	命令用于使用 Dockerfile 创建镜像

> 语法

- 使用当前目录的 Dockerfile 创建镜像

  ```shell
  docker build [OPTIONS] PATH
  ```

- 使用URL 的 Dockerfile 创建镜像

  ```shell
  docker build [OPTIONS] URL 
  ```

- 也可以通过 -f Dockerfile 文件的位置创建镜像

  ```shell
   docker build [OPTIONS] -f /path/to/a/Dockerfile .
  ```

  . ：表示保存在当前目录下

>OPTIONS（可加参数）

1. [^--build-arg=[\]]: 设置镜像创建时的变量

2. [^-f]: 指定要使用的Dockerfile路径

3. [^--force-rm]: 设置镜像过程中删除中间容器

4. [^-m]: 设置内存最大值

5. [^--tag,|-t:]: 镜像的名字及标签，通常 name:tag 或者 name 格式；可以在一次构建中为一个镜像设置多个标签

6. [...](http://www.runoob.com/docker/docker-build-command.html)








## 镜像的原理

 	它是一种联合文件系统，也就是类似于文件夹，一个一个子目录嵌套成一个完整的目录。放在镜像当中理解就是，第一层镜像就好似一个根目录，在子下存在多个子镜像。

采用这种架构的好处就是能够**共享资源**。比如：

	有多个镜像都从相同的base镜像构建而来，那么宿主机只需要在磁盘上保存一份base镜像。同时在内存中只需要加载一份base镜像，就可以为所有容器服务了。而且镜像每一层都可以被共享。
	
	在使用中有一个现象，当你一开始下载的时候你会觉得特别慢，但到之后下的时候，你就会发觉到很快，就是因为镜像这种架构的存在。





##容器数据卷

###介绍

	Docker容器产生的数据，如果不同docker commit生成新的镜像，使得数据作为镜像的一部分保存下来。

那么当容器关闭，随即也会将产生的数据删除。

**1.容器数据卷能够将数据进行持久化，将其映射到主机目录上，实现容器资源与主机共享。**

**2.并使之也能够在容器间进行数据共享**

###实现

> -v参数命令创建

- 不带权限（容器可读读写）

```shell
docker run|create -v /宿主机目录绝对路径：/容器内目录 镜像ID
```



- 带权限（容器只读不能写）

```shell
docker run|create -v /宿主机目录绝对路径：/容器内目录:ro 镜像ID
```



> 使用Dockerfile创建

1. 创建Dockerfile文件

   ```shell
   FROM centos
   VOLUME ["容器数据卷目录1"，"容器数据卷目录2"]
   CMD echo "finish,-----success"
   CMD /bin/bash
   ```

2. 使用docker bulid命令构建新的镜像

###查看数据卷是否挂在成功

创建之后就会在宿主机和容器下的系统个创建一个数据卷目录。

可通过docker inspect查看容器完整信息。



## Dockerfile

### 介绍

	Dockerfile是用来构建Docker镜像的构建文件，用Java的思维想，就是如果需要创建一个类的实例，首先必须要有一个类的class声明，这个class默认的会继承一个Object类，之后在根据开发者的需求再往class中添加各种属性，最终通过new语法创建出了一个实例对象。
	
	在镜像层面上理解的话，同样都属于构建文件，也就是所有的镜像是起源于一个基础镜像，之后根据开发者的需求在往环境添加工具，比如说jdk、mysql、tomcat...。最终通过bulid命令创建出一个可使用的镜像。

[^scratch]: 所有镜像起源镜像，类似于Object

### bulid构建过程

1. docker从基础镜像运行一个容器
2. 执行一条执行并对容器做出修改
3. 执行类似docker commit的操作提交一个新的镜像层
4. docker 再基于刚提交的容器运行一个新容器
5. 从第二部开始重新对新容器执行dockerfile中的下一条指令，直到所有指令都执行完成

### Dockerfile保留字指令

	下面说到的命令的执行，这些命令的执行是在类似于linux终端上执行的，比如：ls（列出当前所有目录）、pwd（当前所在命令）、yum...（软件包操作命令） 等...

- FROM（from）

  	基础镜像

- MAINTAINER（maintainer）

  	作者


- ENV（env）

  	环境变量，这个环境变量可以在后续的任何RUN等其他指令中使用，如：

  ```SHELL
  ENV	MY_PATH /root
  WORKDIR $MY_PATH
  ```

  	当设置JDK等的环境变量也需要使用到该指令

- RUN（run）

  	**容器构建时**在新构建后运行的容器上执行的命令

- EXPOSE（expose）

  	暴露的端口	

- WORKDIR（workdir）

  	工作目录，也就是进入容器后通过pwd查看到目录

- ADD（add）

  	往容器添加文件，若是压缩文件的话，就会解压

- COPY（copy）

  	同样也是往容器添加文件，但也是压缩文件的话，不会解压

- VOLUME（volume）

  	容器数据卷映射名

- CMD（cmd）

  	**镜像构建之后运行的容器启动时**需要执行的命令，Dockerfile中可以存在多条CMD指令，那么最后一条也就是最终构建出来的镜像所运行时所执行的命令。

  若容器运行时添加了运行参数命令，优先执行该命令，不执行Dockerfile中的CMD命令

- ENTRYPOINT（entrypoint）

  	同样也是**容器启动时**需要执行的命令，与CMD不同的是，

  若容器运行时添加了运行参数命令，ENTRYPOINT中的命令同样会得到执行

- ONBUILD（onbuild）

  	若通过该Dockerfile构建出来的镜像之后被作为基础镜所构建，那么当被构建时（docker bulid）将执行ONBULID声明的命令



### 案例

> 构建一个有vim编辑器的centos

- 创建centos_Dockerfile

  ```shell
  FROM centos
  MAINTAINER JR<1336913542@qq.com>
  
  ENV MY_WORKDIR /usr/local
  WORKDIR $MY_WORKDIR
  
  RUN yum -y install vim #下载vim编辑器
  
  EXPOSE 80
  
  CMD echo "BULID SUCCESS-----------SUCCESS!!!"
  CMD /bin/bash
  ```

- 使用docker bulid命令构建新镜像

  ```
  docker build -f /myCentos/centos_Dockerfile -t my_centos .  # .:表示当前目录
  ```

- 结果

  ![docker_build](/photo\docker_build.png)




	

## 安装并运行Redis

1. **下载Redis镜像**

   ```shell
   docker push redis #默认下载最新版本
   ```

2. **创建并运行Redis容器**

   ```shell
   docker run 					#创建并启动容器
   -p 6379:6379 				#容器端口映射
   -v /JR/myredis/data:/data 	#映射容器数据卷
   -v /JR/myredis/conf/redis.conf:usr/local/etc/redis/redis.conf
   -d 							#后台运行
   redis:tag 					#指定镜像名
   redis-server /usr/local/etc/redis/redis.conf --appendonly yes  #执行命令和启动参数
   ```

   **提示**

   ​	映射数据卷容器中的data文件是redis中持久化文件，也就是数据从内存持久化到磁盘中的数据，比如set k1 v1，那么最终这些数据保存到磁盘中就是再该目录下。

   ​	映射的redis.conf文件的好处是，如果我们需要修改容器内的redis软件的配置，则不需要进入到容器内修改，而是直接再宿主机的映射目录下修改即可。

   	redis-server是启动容器时的执行命令，表示启动容器后，再容器系统内启动redis服务。

3. **进入容器内的redis客户端**

   ```shell
   docker exce				 #进入容器内执行命令
   --it					#开启终端运行容器
   容器ID	 	 		 #真在运行的容器ID
   redis-cli			#执行命令（开启客户端）
   ```




