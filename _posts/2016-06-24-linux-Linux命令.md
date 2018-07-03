---
layout: post
title:  "Linux命令"
date:   2016-06-24
categories: Linux
---
### 基础命令

```
$ cat /etc/redhat-release		# 查看系统版本号
$ uname –r						# 查看内核版本号
$ uname –m						# 查看系统架构
$ whoami							# 查看系统的当前用户
```

### 命令行命令
[Linux Tools](http://linuxtools-rst.readthedocs.org/zh_CN/latest/tool/ps.html)

[一介布衣教程](http://yijiebuyi.com/)
```
查看端口占用
lsof -i tcp:port

查看程序端口
ps -ef | grep Name

查看文件信息输出，档案有更新会主动刷新。
tail - - - 

配置环境变量
* vi ~/.zshrc文件
* export PATH="$HOME/.composer/vendor/bin:$PATH"
* source ~/.zshrc
* echo $PATH
```

（第三方问卷、语音任务、照相任务）
cat  log/production.log | grep -C 5 'obYTojqx6PDDzFhFlZ-0tfa6t5MQ' 显示file文件里匹配foo字串那行以及上下5行

### 用户相关

```
adduser wangjing  #添加wangjing用户
添加wangjing的sudor权限  vi /etc/sudoer
```

### 网络相关

#### Curl命令

curl是利用URL语法在命令行方式下工作的开源文件传输工具。在Unix、多种Linux发行版和DOS、Win32、Win64下的移植版本中广泛使用。

常用命令

```
下载单个文件，默认将输出打印到标准输出中(STDOUT)
curl http://easyer.github.io
通过-o/-O选项保存下载的文件到指定的文件中；
-o:将文件保存到指定文件
-O:使用URL中默认的文件名保存文件到本地
curl -o test.html http://easyer.github.io
获取多个文件
curl -O URL1 -O URL2
通过-L选项进行强制重定向
curl -L http://www.google.com
断点续传：通过添加-C选项可以继续对该文件进行下载，已经下载过的文件不会被重新下载
curl -C - -O http://www.gnu.org/software/gettext/manual/gettext.html
对Curl使用网络限制:下载速度最大不超过1000B/second
curl --limit-rate 1000B -O http://www.gnu.org/software/gettext/manual/gettext.html
设置一个日期，与文件最后修改日期比较，如果文件在指定日期后修改过就下载，否则不下载
若yy.html文件在2011/12/21后有过更新才进行下载
curl -z 21-Dec-11 http://www.example.com/yy.html
授权：在访问需要授权的页面，通过-u提供用户名和密码进行授权
curl -u username:passwd URL #也可curl -u username URL 提示输入密码
从ftp服务器下载文件
列出public_html下的所有文件夹和文件
curl -u ftpuser:ftppass -O ftp://ftp_server/public_html/
下载xss.php文件
curl -u ftpuser:ftppass -O ftp://ftp_server/public_html/xss.php
上传文件到ftp服务器
将myfile.txt文件上传到服务器
curl -u ftpuser:ftppass -T myfile.txt ftp://ftp.testserver.com
同时上传多个文件
curl -u ftpuser:ftppass -T "{file1,file2}" ftp://ftp.testserver.com
从标准输入获取内容保存到服务器指定的文件中
curl -u ftpuser:ftppass -T - ftp://ftp.testserver.com/myfile_1.txt
为Curl设置代理
curl -x proxyserver.test.com:3128 http://google.co.in
保存于使用网站cookie信息
将网站的cookies信息保存到sugarcookies文件中
curl -D sugarcookies http://localhost/sugarcrm/index.php 
使用上次保存的cookie信息
curl -b sugarcookies http://localhost/sugarcrm/index.php
传递请求数据
get请求
curl -u username https://api.github.com/user?access_token=xxxxx
post请求,通过--data/-d方式指定使用post方式传递数据
curl -u username --data "param1=value1&params2=value2" https://api.github.com
也可以指定一个文件，将该文件中的内容作为数据传递给服务端
curl --data @filename https:/github.api.com/authorizations
#--data--urlencode 该选项会自动转移参数中的特殊字符
curl -I -X DELETE https://api.github.com 通过-X指定其他协议
上传文件
curl --form "fileupload=@filename.txt" http://hostname/resource
```

ping 和 traceroute 命令。

### HomeBrew工具
* brew unlink xxx  取消与xxx的关联



（第三方问卷、语音任务、照相任务）
cat  log/production.log | grep -C 5 'obYTojqx6PDDzFhFlZ-0tfa6t5MQ' 显示file文件里匹配foo字串那行以及上下5行

### Linux基本命令

```
$ top   # 查看系统状态
top - 09:36:03 up 5 days, 39 min,  2 users,  load average: 0.01, 0.04, 0.05
# 第一行，任务队列信息，同uptime执行结果一样，显示的内容依次为“系统当前时间 、系统到目前为止已运行的时# 间、当前登录系统的用户数量、系统负载(任务队列的平均长度)三个值分别为1分钟、5分钟、15分钟前到现在的平# 均值【这三个一般会小于1，如果持续高于5，请仔细查看那个程序影响系统的运行】”
Tasks: 138 total,   1 running, 137 sleeping,   0 stopped,   0 zombie
# 第二行，Tasks — 任务（进程），“所有启动的进程数”、“正在运行的进程数”、“挂起的进程数”、“停止的进程# # 数”、“僵尸进程数”。
%Cpu(s):  0.3 us,  0.3 sy,  0.0 ni, 99.3 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
# 第三行，cpu状态信息
# 0.1%us — 用户空间占用CPU的百分比
# 0.0% sy — 内核空间占用CPU的百分比
# 0.0% ni — 改变过优先级的进程占用CPU的百分比
# 99.9% id — 空闲CPU百分比
# 0.0% wa — IO等待占用CPU的百分比
# 0.0% hi — 硬中断（Hardware IRQ）占用CPU的百分比
# 0.0% si — 软中断（Software Interrupts）占用CPU的百分比
# 0.0% st —  Steal Time
KiB Mem :  1879452 total,   363596 free,   194496 used,  1321360 buff/cache
# 第四行,内存状态
# 16321212k total — 物理内存总量
# 15818264k used — 使用中的内存总量
# 502948k free — 空闲内存总量
# 1843872k buffers — 缓存的内存量
KiB Swap:  2064380 total,  2064380 free,        0 used.  1408724 avail Mem
# 第五行，swap交换分区信息
# 8314876k total — 交换区总量
# 0k used — 使用的交换区总量
# 8314876k free — 空闲交换区总量
# 13291436k cached — 缓冲的交换区总量
剩下的部分是各进程的状态监控项目，项目列表。
```


### VIM命令

```
$ :%s/1/2/g   # 替换文本，1替换为2
```

### Postgresql操作

```
$ psql -Uroot postgres 		# 登录root是账号，postgres是默认数据库
postgres=# \h					# psql内部查看帮助
postgres=# create database "pdca_api_feature";  # 创建数据库

$ pg_restore -Uroot -d pdca_api_feature -Ft pdca.tar  # 根据备份恢复数据

【PostgreSQL】如何删除还有活动链接的数据库
postgres=# SELECT pg_terminate_backend(pg_stat_activity.pid) FROM pg_stat_activity  WHERE datname='testdb' AND pid<>pg_backend_pid();
 执行上面的语句之后，在执行DROP操作，就可以删除数据库了。

* 语句说明：
pg_terminate_backend：用来终止与数据库的连接的进程id的函数。
pg_stat_activity：是一个系统表，用于存储服务进程的属性和状态。
pg_backend_pid()：是一个系统函数，获取附加到当前会话的服务器进程的ID。


```























