---
layout: post
title:  "Nginx介绍"
date:   2016-03-24
categories: Web
---

#Nginx介绍

[Nginx开发从入门到精通](http://tengine.taobao.org/book/index.html) |
[Nginx开发从入门到精通 - github](https://github.com/taobao/nginx-book)


Nginx，一个轻量级高性能的HTTP服务器和反向代理服务器软件。还是一个IMAP/POP3/SMTp代理服务器。
Nginx以其高性能，稳定性，丰富的功能，简单的配置和低资源消耗而闻名。

几大优点(对比Apache)

1. 性能上：占用很少的系统资源，支持更多的并发连接，达到更高的访问效率；
2. 功能上：Nginx是优秀的代理服务器和负载均衡服务器；
3. 安装配置上：Nginx安装简单、配置灵活；

相比其他Web服务器的优势：

1. 作为Web服务器，Nginx处理静态文件、索引文件，自动索引的效率非常高；
2. 作为代理服务器，Nginx可以实现无缓存的反向代理加速，提高网站运行速度；
3. 作为负载均衡服务器，Nginx既可以在内部直接支持Rails和PHP，也可以支持HTTP代理服务器对外进行服务，同事还支持简单的容错和利用算法进行负载均衡；
4. 在性能上，Nginx是专门为性能优化而开发的，在实现上非常注重效率。采用内部Poll模型，可以支持更多的并发连接，最大可以支持50000个并发连接数的响应，而且占用很低的内存资源；
5. 在稳定性方面，Nginx采用了分阶段资源分配技术，使得CPU与内存的占用率非常低；
6. 在高可用性方面，Nginx支持热部署，启动速度特别迅速，可以在不间断服务的情况下，对软件进行升级；

### Nginx模块和工作原理
Nginx由内核和模块组成。

```
模块
结构上：核心模块、基础模块和第三方模块。
功能上：Handlers(处理器模块)、Filters(过滤器模块)、Proxies(代理类模块)。
```
1. Handlers(处理器模块):直接处理请求并输出内容和修改headers等操作。Handlers处理器模块一般只能有一个。
2. Filters(过滤器模块):对其他处理器模块输出的内容进行修改操作，最后由nginx输出。
3. Proxies(代理类模块):是Nginx的HTTP Upstream之类的模块，这些模块与后端一些服务比如FastCGI等交互，实现服务器代理和负载均衡等功能。




### Nginx配置

```
主要配置文件/etc/nginx/nginx.conf
nginx -V 可以查看安装Nginx时的编译选项。
```

```
层级结构
- main(全局设置)
  - Events
  - HTTP
    - server block 1(主机设置)
      - location 1(Url匹配特定位置的设置)
      - location 2
    - server block 2
      - location 1
      - location 2
  - upstream (负载均衡)

关系：server继承main，location继承server，upstream既不集成其他设置也不被其他继承。
```

```
Nginx全局配置
user wangjing wangjing;
worker_processes 4;
pid /var/run/nginx.pid;
events {
	use epoll;
	worker_connections 768;
}
user是主模块命令，用来指定nginx worker进程运行用户及用户组，默认是nobody；
worker_precesses是主模块指令，指定nginx要开启的进程数,一个进程平均耗费10~12MB内存，一般按照CPU核书数指定进程数即可；
pid是主模块指定，用来指定进程id的存储文件位置；
events指令用来指定nginx的工作模式及连接数上限；
use是个事件模式指令，用来指定nginx的工作模式。nginx支持工作模式有select/poll/kqueue/epoll/rtsig和/dev/poll,其中select和poll是标准工作模式，kqueue和epoll是高效的工作模式，不同的是epoll用在linux平台，kqueue用在BSD系统。对于Linux，epoll是首选。
worker_connections是个事件模块指令，指定nginx每个进程的最大连接数，默认是1024。最大客户端连接数为max_clients=worker_processes*worker_connections;作为反向代理时，max_clients=worker_processes*worker_connections/4;

进程的最大连接数受Linux系统进程的最大打开文件数限制，在执行操作系统命令"ulimit -n 65536"后，worker_connections的设置才生效。
```

```
HTTP服务器配置
http{
	include /etc/nginx/mime.types;
	default_type application/octet-stream;
	log_fromat main '...'
	client_max_body_size 20m;
	client_header_buffer_size 32k;
	large_client_header_buffers 4 128k;
	sendfile on;
	tcp_nopush on;
	tcp_nodelay on;
	keepalive_timeout 60;
	client_header_timeout 10;
	client_body_timeout 10;
	send_timeout 10;
}
include是个主模块指令，实现对配置文件所包含的文件的设定，可以减少主配置文件的复杂度。
default_type属于HTTP的核心模块指令，这里设置默认类型为二进制流，也就是说当文件类型未设置时，使用这种方式；
log_format是Nginx的HttpLog模块指令，用于指定Nginx日志的输出格式。main是次日志的输出格式的名称。可以在下面的access_log指令中引用。
client_max_body_size用来设置允许客户端请求的最大的单个文件字节数。
client_header_buffer_size用来指定来自客户端请求头的headerbuffer大小，对于大多数请求，1k足够，如果自定义了消息头部或者有更大的cookit，可以增加缓存大小，这里设置32k。
large_client_header_buffers用来指定客户端请求中较大的消息头的最大数量和大小，4为个数，128为大小，最大缓存为4个128kb。
sendfile用于开启高效文件传输模式。将tcp_nopush和tcp_nodelay两个指令设置为on，用于防止网络阻塞。
keepalive_timeout用于设置客户端连接保持活动的超时时间。在超过这个时间之后，服务器会关闭该连接。
client_header_timeout用于设置客户端请求头读取超时时间。如果超过这个时间，客户端还没有发送任何数据，nginx将返回“Request out(408)”错误。
client_body_timeout用于设置客户端请求主体读取时间，默认是60，如果超过这个时间客户端还没有发送任何数据，nginx将返回“Request time out(408)”错误。
send_timeout用于指定响应客户端的超时时间。这个超时仅限于两个连接活动之间的时间，如果超过这个时间客户端没有任何活动，nginx将会自动关闭。
```

```
其它模块HttpGzip
配置
```

```
负载均衡配置
upstream ixdba.net{
	ip_hash;
	server 192.168.12.133:80;
	server 192.168.12.134:80 down;
	server 192.168.12.135:8009 max_fails=3 fail_timeout=20s;
	server 192.168.12.136:8080;
}
upstream是ngxin的HTTP Upstream模块，这个模块通过简单的调度算法实现客户端IP到后端服务器的负载均衡。
ixdba.net是负载均衡的名称，这个名称可以任意指定。
在HTTP Upstream模块中，可以通过server指令指定后端服务器的IP地址和端口，同时还可以设定每个后端服务器在负载均衡调度中的状态，常用状态：
down 表示当前的server暂不参与负载均衡；
backup 预留的备份机器。其他非backup机器出现故障或者忙碌的时候，才请求此机器，so这台机器压力最小；
max_fails 允许请求失败的次数，当超过最大次数时，返回proxy_next_upstream模块定义的错误；
fail_timeout 在max_fails次失败后，暂停服务的时间，和max_fails一起使用；
```

```
server虚拟主机配置
server{
	listen 80;
	server_name 192.168.12.188 www.ixdba.com;
	index index.html index.htm;
	root /web/www/www.ixdba.net;
	charset gb2312;
}
server标志定义虚拟主机开始：listen用于指定虚拟主机的服务端接口；server_name用来指定IP地址或者域名，多域名之间用空格分开；index指定默认访问页；root指令用于指定虚拟主机的网页根目录，这个目录可以是相对路径，也可以是绝对路径；charset用于设置网页的默认编码格式；
access_log logs/www.ixdba.net.access.log main;
access_log用于指定此虚拟主机的访问日志存放路径。最后的main用于指定访问日志的输出格式。
```

```
URL匹配设置
是Nginx配置中最灵活的部分。location支持正则表达式，也支持条件判断匹配用户可以通过location指令实现Nginx对动、静态网页的过滤处理。
```

























