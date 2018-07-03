
Docker是一个开源的应用容器引擎，开发者可以利用Docker打包自己的应用以及依赖包到一个可移植的容器中，然后发布到任何流行的Linux机器上，也可以实现虚拟化。容器完全是沙箱机制，相互之间不会有任何接口(类似iPhone的app)。几乎没有性能开销，可以很容易的在机器和数据中心运行。最重要的是，他们不依赖于任何语言、框架或包括系统。

解决运行环境和配置问题，方便发布，(也就方便做持续集成)。<br>
做了良好的资源隔离(能够防止某个程序占用太多内存)。<br>
更新统一性(所有镜像都是基于父镜像的,父镜像只有一个,容器就是类似于镜像实例的东西,整体配置的改变只需要修改相应的镜像即可)。



[Docker技术入门与实战](http://book.51cto.com/art/201502/466244.htm)-什么是Docker-为什么要使用Docker<br>
[理解Docker](http://segmentfault.com/a/1190000002609286)<br>
[快速理解Docker](http://blog.csdn.net/colorant/article/details/20608157)<br>
[Docker教程](http://www.widuu.com/docker/docker.html)<br>
[Docker使用基本教程](http://docker.widuu.com/)<br>
[Docker Hub](http://www.oschina.net/news/57894/daocloud)<br>
[深入浅出Docker1-6](http://www.infoq.com/cn/articles/docker-core-technology-preview?utm_source=infoq&utm_medium=related_content_link&utm_campaign=relatedContent_articles_clk)<br>


**历史**

Docker是PaaS提供商dotCloud开源的一个基于LXC的高级容器引擎，源代码托管在Github上，基于go语言并遵从Apache2.0协议开源。Docker提供了一种在安全、可重复的环境中自动部署软件的方式，她的出现拉开了基于云计算平台发布产品方式的变革序幕。

Docker自2013年以来非常火热，无论是从github上的代码活跃度，还是Redhat在RHEL6.5中集成对Docker的支持，就连Google的Compute Engine也支持docker在其之上运行。


1. 简介
2. 安装Docker
3. Docker入门
4. 使用Docker镜像和仓库
5. 在测试中使用Docker
6. 使用Docker构建服务
7. 使用Fig编配Docker
8. 使用Docker API
9. 获得帮助和对Docker进行改进

PaaS，Platform as a Service,平台即服务。把服务器平台作为一种服务提供的商业模式。通过网络进行程序提供的服务称之为SaaS(Software as a Service),而云计算时代相应的服务器平台或者开发环境作为服务进行提供就成为了Paas(Platform as a Service)。

容器是直接运行在操作系统内核之上的用户空间，只能运行与底层宿主相同或相似的系统，这看起来非常不灵活。

**Docker的核心组件：**

* Docker客户端和服务器
* Docker镜像
* Registry
* Docker容器

**Docker能做什么**

Docker可以通过对应用组件的封装(Packaging)、分发(Distribution)、部署(Deployment)、运行(Runtime)等生命周期的管理，达到应用组件级别的"一次封装，到处运行".这里的应用组件，既可以是一个Web应用，也可以是一套数据库服务，甚至是一个操作系统或编译器。

**安装Docker前提**

* 64位架构计算机
* 运行Linux 3.8或者更高版本的内核。
* 内核必须支持一种适合的存储驱动(storage driver):Device Manager、AUFS、vfs、btrfs。默认存储驱动通常是Device Manager。
* 内核必须支持并开启cgroup和命名空间(namespace)功能。

**Docker安装用到的命令(Ubuntu)**

添加仓库并自动将仓库的GPG添加到宿主机中：
sudo sh -c "echo https://get.docker.io/ubuntu docker main >/etc/apt/source.list.d/docker.list" <br>
确认是否安装了curl：whereis curl <br>
更新ADT源：sudo apt-get update <br>
安装Docker软件包：sudo apt-get install lxc-docker <br>
确认Docker是否正常运行：sudo docker info <br>
Docker与UFW。。。。。。。

### Docker入门

三个基本概念：

* 镜像（Image）
* 容器（Container）
* 仓库（Repository）

[mac安装Docker](http://www.tuicool.com/articles/2Qzq2y) \|
[mac安装docker问题解决](https://www.embbnux.com/2015/09/04/mac_osx_cannot_boot_docker_default_machine_problem_solve/) \|
[brew安装docker2docker问题](http://www.cnblogs.com/web2-developer/archive/2016/03/19/docker-1.html) \|

**sudo docker info**该命令返回所有容器和镜像(镜像即Docker用来构建容器的"构建块")的数量,Docker使用的执行驱动和存储驱动(execution and storage driver),以及Docker的基本配置。

Docker是基于客户端和服务器架构的。它有一个docker程序，既能作为客户端也能作为服务端。作为客户端时，docker程序向Docker守护进程发送请求(如请求返回守护进程自身的信息)，然后再对返回的请求结果进行处理。

**docker search 镜像名字**搜索可用docker镜像:docker search tutorial

**docker pull 镜像名字**下载容器镜像:docker pull learn/tutorial

**docker run 镜像名 镜像中运行的命令**运行docker中命令:docker run learn/tutorial echo "hello world"

**docker run 镜像名 镜像中运行命令**在容器中安装新的程序:docker run learn/tutorial apt-get install -y ping.<br>
备注：apt-get命令执行完毕之后，容器会停止，但对容器的改动不会丢失。在执行apt-get命令的时候，要带上-y参数，如果不指定参数，apt-get命令会进入交互模式，需要用户输入命令来进行确认，但在docker环境中是无法响应这种交互的。

**docker commit ID name**保存对容器的修改:docker commmit 698 learn/ping.<br>
提示：<br>
1. 运行docker commit,可以查看该命令的参数。<br>
2. 你需要指定要提交保存容器的ID。(可以通过docker ps -l命令获得)<br>
3. 无需卡贝完整的id，通常用最开始的三至四个字母区分即可。(非常类似git里面的版本)

**运行新的镜像**同运行docker中的命令:docker run learn/ping ping www.baidu.com

**docker inspect ID**检查运行中的镜像(用docker ps可以查看所有正在运行的容器的列表)

**docker push learn/ping**发布自己的镜像<br>
1. docker images命令可以列出所有安装过的镜像。
2. docker push命令可以将某一个镜像发布到官方网站。<br>
3. 只能将镜像发布到自己的空间下。当前登录的是learn账号。(类似于github)


**docker run**命令提供了容器Docker容器创建到启动的功能。

**sudo docker run -i -t ubuntu /bin/bash**创建第一个容器

docker命令

docker rm id 删除容器
docker rm id 删除镜像

## Docker使用场景
Docker的真实使用场景分别是简化配置、代码流水线管理、提高开发效率、隔离应用、整合服务器、调试能力、多租户环境、快速部署。Docker提供了轻量级的虚拟化，它几乎没有任何额外开销，这个特性非常酷。首先你在享有Docker带来的虚拟化能力的时候无需担心它带来的额外的开销。其次，相比于虚拟机，你可以在同一台机器上创建更多数量的容器。另一个优点就是启动与停止都能在几秒内完成。
### 简化场景
这是Docker公司宣传的Docker的主要使用场景。

[八个Docker的真实应用场景](http://dockone.io/article/126)


#Docker学习

### 安装
```
docker is configured to use the default machine with IP 192.168.99.100
For help getting started, check out the docs at https://docs.docker.com

如果报找不到ca.pem问题，通过
docker-machine env #将环境变量设置一下 
docker命令就可以使用了

阿里:
http://mirrors.aliyun.com/help/docker-toolbox
```

### Docker基本命令
```
docker #查看docker的常用命令
docker ps #查看正在运行的容器 组合如下：
-a : 查看所有容器，包括已经停止的
-l : 查看刚刚启动的容器
-q : 只显示容器的ID
-l -q : 返回刚启动容器的ID
docker stop/start/restart
```

Docker主要概念就是镜像和容器。我理解的两者是镜像是静态的，容器是镜像的一个实例。

Docker包含客户端和服务端。docker服务端是一个服务进程，管理所有的容器。docker客户端则扮演着服务端的远程控制器，可以用来控制docker的服务端进程。通常两者运行在同一台机器上。


[Docker快速构建Rails开发环境](https://www.embbnux.com/2016/02/21/docker_for_rails_development_with_postgresql_and_redis/)

[docker compose](https://github.com/docker/compose)









