## Docker安装

[Docker实践系列文章](https://segmentfault.com/a/1190000006449675)

[Docker——从入门到实践](https://yeasy.gitbooks.io/docker_practice/content/compose/introduction.html)

Docker是一个开源工具，它可以让创建和管理Linux容器变得简单。容器就像是轻量级的虚拟机，并且可以以毫秒级的速度来启动和停止。Docker帮助管理员和程序员在容器中开发应用程序，并且可以扩展到成千上万的节点。

容器和VM（虚拟机）的主要区别是，容器提供了基于进程的隔离，而虚拟机提供了资源的完全隔离。虚拟机可能需要1分钟来启动，而容器只要一秒甚至更短。容器使用宿主操作系统的内核，而虚拟机使用独立的内核。

Docker的局限性之一是它只能运行在64位操作系统上。

Docker Engine改为Docker CE（社区版）
包含了CLI客户端、后台进程/服务以及API。

Docker Data Center改为Docker EE（企业版）
付费的Docker服务。

不管是CE还是EE都不影响Docker Compose以及Docker Machine的使用。

现在Docker版本基于YY.MM发布，如17.03就是2017年3月发布的，一般指向17.03.0，如果有bug/安全修复需要发布，将会指向17.03.1等等（类似于Ubuntu）。用户可以选择Stable（发布较慢）或者Edge（发布较快）版本。

Edge版本每月发布，提供一个月支持。Stable每个季度发布一次，提供4个月支持。

#### CentOS安装Docker
[参考](https://docs.docker.com/engine/installation/linux/docker-ce/centos/#set-up-the-repository)

```
$ sudo yum remove docker \
                  docker-common \
                  docker-selinux \
                  docker-engine		# 卸载旧版本的docker
$ sudo yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2										# 设置repository安装docker
$ sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
    										# 设置stable的repository
$ sudo yum install docker-ce			# 安装docker ce
$ sudo systemctl start docker 		# 启动docker
$ sudo systemctl stop docker 			# 停止docker
$ docker info		# 检查docker安装成功
错误：‘Got permission denied while trying to connect to the Docker daemon socket atunix:///var/run/docker.sock: Get http://%2Fvar%2Frun%2Fdocker.sock/v1.26/images/json: dial unix /var/run/docker.sock: connect: permission denied’
是指当前用户没有操作docker的权限，我的解决方案是将当前用户（virgil）加入‘/var/run/docker.sock’的权限组（root）中,然后重启：
$ sudo gpasswd -a virgil docker


    
$ yum install docker -y		# 安装docker
//旧时的sysv语法
$ service docker start		# 启动docker服务
$ chkconfig docker on		# 设置docker开机启动
//CentOS7的新式systend语法
$ systemctl start docker.service	# 启动服务
$ systemctl enable docker.service #设置docker开机启动


```


## Docker02

查看Docker daemon是否运行 $ ps aux | grep docker

#### Docker安装
下载docker for mac

[docker使用阿里云镜像仓库](http://blog.csdn.net/qq_30259339/article/details/52400673)

[Docker 常用命令](https://www.cnblogs.com/me115/p/5539047.html)

[Docker 常用命令详解](http://blog.csdn.net/permike/article/details/51879578)

### Docker常用命令

$ docker images 列出机器上的镜像
$ docker run -i -t <image_name/continar_id> /bin/bash
$ docker run -d -it  image_name
##### 附着到容器
* docker attach <id、container_name>		# 附着到正在运行的容器
* docker exec -t -i <id/container_name>  /bin/bash    # 进入正在运行的容器，同时运行bash（比attach更好用）

##### 查看容器日志
* $ docker logs <id/container_name>  查看容器日志
* $ docker logs -f <id/container_name> (类似 tail -f) (带上时间戳-t）  实时查看日志输出

##### 查看容器
* $ docker ps  列出正在运行的container
* $ docker ps | less -S   #用一行列出所有正在运行的container（容器多的时候非常清晰）
* $ docker rm Name/ID 		#删除单个容器
* $ docker commit ID new_image_name  # 保存对容器的修改（commit） 当你对某一个容器做了修改之后（通过在容器中运行某一个命令），可以把对容器的修改保存下来，这样下次可以从保存后的最新状态运行该容器。

### 阿里云管理Docker镜像
[参考](https://cr.console.aliyun.com/?spm=5176.2020520152.210.d103.690ac26byOvGIg#/dockerImage/97353/detail)

```
开通阿里云镜像服务
登录阿里云docker registry
$ docker login --username=wangjing985@googlemail.com registry.cn-hangzhou.aliyuncs.com
$ docker tag [ImageId] registry.cn-hangzhou.aliyuncs.com/virgil/my-docker:[镜像版本号]  制作要上传的镜像
$ docker push registry.cn-hangzhou.aliyuncs.com/virgil/my-docker:[镜像版本号]    将制作的镜像推送到阿里云

```

### 包含rails环境的Docker
[Centos7 yum安装nginx](http://blog.csdn.net/ysydao/article/details/51388385)

```
1、安装rvm和ruby   -》 ruby2.4.1
2、安装nginx，
```

### 配置nginx的docker环境

```
启动容器
$ docker run -p 8093:80 --name mynginx  -v $PWD/conf/nginx.conf:/etc/nginx/nginx.conf -v $PWD/www:/opt/nginx/www -v $PWD/log:/opt/nginx/log  -d nginx
命令说明
-p 8093:80：将容器的80端口映射到主机的8093端口
  -name mynginx：将容器命名为mynginx
  -v $PWD/conf/nginx.conf:/etc/nginx/nginx.conf：将主机中当前目录下的nginx.conf挂载到容器的/etc/nginx/nginx.conf
  -v $PWD/www:/opt/nginx/www：将主机中当前目录下的www挂载到容器的/opt/nginx/www，参考nginx.conf的root配置
  -v $PWD/log:/opt/nginx/log：将主机中当前目录下的log挂载到容器的/opt/nginx/log，参考nginx.conf的log配置


Docker报错 WARNING: IPv4 forwarding is disabled. Networking will not work.
解决办法：
$ vim  /usr/lib/sysctl.d/00-system.conf
添加如下代码：
 net.ipv4.ip_forward=1
重启network服务
$ systemctl restart network
完成以后，删除错误的容器，再次创建新容器，就不再报错了。


```
























