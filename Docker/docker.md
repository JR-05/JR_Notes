## 介绍

	Docker是一种类似于与虚拟机技术，是一种虚拟容器技术。
	
	是一个容器运行载体或称之为容器管理引擎。我们把应用程序和配置依赖打包好形成一个可交付的运行环境，这个打包好的运行环境就似乎image镜像文件。
	
	但与虚拟机技术不同的是，在Docker容器上运行的程序并不是一个完整的操作系统+软件，Docker舍弃了传统虚拟机容器中需要的Hypervisor硬件虚拟化，运行在Docker容器上的程序直接使用实际物理机的硬件资源。因此在CPU，内存利用率上Docker将会在效率上有明显优势。并且Docker的镜像文件也比平常的镜像文件小很多。

### Docker三要素

	Docker的组成架构
	
	#### 仓库
	
	是集中存放镜像文件的场所 

#### 镜像

	它可以看作是一个轻量级的操作系统+应用程序，但它省去了硬件Hypervisor虚拟化，而又它只能运行在Docker引擎内。
	
	我们把应用程序和配置依赖打包好形成一个可交付的运行环境，这个打包好的运行环境就似乎image镜像文件。

#### 容器

	我们通过这个镜像文件生成的实例称为容器，image镜像文件可以看作是容器的模板。同一个image文件可以生成多个同时运行的容器实例。
	
	就好比class类可以生成多个实例对象。



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

	注意，Docker命令必须在管理员权限下执行



[^docker help]: 查看所有Docker命令及解释

![docker_help](C:\Users\Administrator\Desktop\JR\markdown\notes\Docker\photo\docker_help.png)



[^docker info]: 查看当前Docker信息，包括所有镜像文件，当前正在运行容器，关闭容器，以及Docker引擎运行环境...

![docker_info](C:\Users\Administrator\Desktop\JR\markdown\notes\Docker\photo\docker_info.png)



[^docker images]: 查看本地仓库下所有的镜像文件

![docker_images](C:\Users\Administrator\Desktop\JR\markdown\notes\Docker\photo\docker_images.png)

- 标记

1. REPOSITORY ：镜像仓库源，可用于操作镜像命令用的

2. TAG：镜像标签，比如同一个Mysql镜像有多个版本，那么TAG就是用于区分不同版本的

3. IMAGE ID : 区分本地不同镜像ID，可用于操作镜像命令用的

4. CREATED：镜像在本地创建时间

5. SIZE：镜像大小

- 可加参数

1. docker images -a :列出本地所有镜像

2. docker images -q :只显示镜像ID
3. docker images --digests：显示镜像的摘要信息
4. docker images --no-trunc：显示完整的镜像信息

**可追加参数**

[^docker images -aq]: 显示所有镜像的ID

![docker_images -aq](C:\Users\Administrator\Desktop\JR\markdown\notes\Docker\photo\docker_images -aq.png)

[^docker imgaes -a --no-trunc]: 显示所有镜像的完整信息

![docker_images -a --no-trunc](C:\Users\Administrator\Desktop\JR\markdown\notes\Docker\photo\docker_images -a --no-trunc.png)













































