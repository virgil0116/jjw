
---
layout: post
title:  "Ubuntu做服务器部署Rails应用"
date:   2016-03-23
categories: Ruby
---
# Ubuntu做服务器
**主要是服务器的部署、配置。同时包含一些操作命令的记录**

```
查看系统版本
$ cat /proc/version
$ uname -a
$ lsb_release -a
```
### 在模拟器中安装Ubuntu

1. 更换源：http://mirrors.aliyun.com/ 阿里源
2. 安装ssh-server
3. root安装nginx $ apt-get install nginx -y   (安装完默认启动；启动命令：systemctl start nginx)
4. root安装mysql

```
$ apt-get install mysql-server -y     （安装完默认启动）
$ apt-get install libmysqlclient-dev    (安装组件)
$ mysql -u root -p                    (测试登录成功)
$mysql> show databases;     (显示数据库)
```

5. root安装redis [链接](https://www.cnblogs.com/zongfa/p/7808807.html)

```
$ apt-get install redis-server
$ ps -ef | grep redis
```

6. root安装git  $ apt-get install git -y
7. root安装imagemagick   $ apt-get install imagemagick -y   [链接](https://blog.csdn.net/jacke121/article/details/76126245)
8. root安装nodejs        $ apt-get install nodejs
9. root安装rvm    [链接](http://www.rvm.io/)
10. 添加用户

```
$ adduser www    # 添加www用户
$ vim /etc/sudoers    # 添加一行   ‘www     ALL=(ALL:ALL) ALL’
```

11. 配置免密码登录

```
~$ ssh-keygen -t rsa      # 用户目录执行生成.ssh文件夹
$  vim authorized_keys    # 编辑该文件，将本地的~/.ssh/id_rsa.pub中内容拷贝进去
退出重新登录即可免密码。

方法一：
$ ssh wangjing@192.168.97.134
或者将本机的.ssh/id_isa.pub的值复制到服务器的.ssh/authorized_keys中
方法：
本机：$ ssh-keygen -t rsa (回车生成id_isa.pub)
服务器上复制刚才的值加进authrized_keys中
。
方法二：
$ ssh-keygen  # 生成key
$ ssh-copy-id -i ~/.ssh/id_rsa.pub remote-host  # 将生成的key拷贝到线上服务器，后面指服务器地址
$ ssh remote-host   # 可以无需密码登录
```

12. 修改部署目录权限

```
$ sudo chown www /var/www   # 修改目录为www用户
$ sudo chgrp www /var/www   # 修改目录为www组
这样/var/www 目录就作为项目部署目录。
```



4. [安装nginx](http://www.cnblogs.com/kunhu/p/3633002.html)
[nginx源](http://www.kaijia.me/2013/05/ubuntu-latest-nginx-repo-collection/)

```  
  apt-get install nginx   #不推荐  版本过低
  sudo service nginx start  /(sudo /etc/init.d/nginx start) #不推荐  版本过低

  编译安装[下载](http://nginx.org/download/)
  需要安装一些依赖。根据提示安装
  ~ wget http://nginx.org/download/nginx-1.9.9.tar.gz
  ~ sudo tar -zxvf nginx-1.9.9.tar.gz
  ~ cd nginx-1.8.0
  ~/nginx-1.9.9 sudo ./configure
  ~/nginx-1.9.9 sudo make&&make install

  /usr/local/nginx/sbin ./nginx -v  # nginx version: nginx/1.9.9
  /etc/init.d/nginx start #启动
  /etc/init.d/nginx stop #停止
  /usr/local/nginx/sbin ./nginx #启动
  /usr/local/nginx/sbin ps -ef | grep nginx

```

5. 配置本地。本地/etc/hosts中添加一条记录  
	如：192.168.97.134 www.etan.com  #ip是ubuntu服务器的ip地址，这样就可以用www.etan.com访问ubuntu了。

6. [ubuntu安装部署rails](http://www.jb51.net/article/52155.htm)

	[rvm 安装](http://www.rvm.io/)

	[替换源](https://ruby.taobao.org/)

```
$ source .bashrc
$ source .bash_profile
$ sed -i -e 's/ftp\.ruby-lang\.org\/pub\/ruby/ruby\.taobao\.org\/mirrors\/ruby/g' ~/.rvm/config/db  #替换taobao源
$ rvm install 2.2.1 #安装2.2.1版本ruby
$ gem sources --add https://gems.ruby-china.org/ --remove https://rubygems.org/  #添加rubychina的gem源
$ sudo apt-get install mysql-server  #安装过程要求输入密码：wangjing

```
7. 配置PostgreSQL

[教程](http://blog.sina.com.cn/s/blog_6af33caa0100ypck.html)---
[PostgreSQL源](https://www.postgresql.org/download/linux/ubuntu/)
[教程](https://www.postgresql.org/docs/current/static/sql-alteruser.html)
[设置新用户可以登录](https://blog.csdn.net/wanderman1836/article/details/71173030)
```
$ psql -U user_name postgres -h 127.0.0.1  #登录
postgres=# create user “zhaofeng” with password ‘123456’ nocreatedb; #创建用户，注意用户名要用双引号，区分大小写，密码不用
postgres=# create database “testdb” with owner=”zhaofeng”; #创建数据库并指定所有者
查看版本
$ psql --version
$ psql -U postgres postgres #登录
$ sudo -u postgres createdb weizhu_zhongfu -O root -E UTF8 -e   # 手动创建名为weizhu_zhongfu的数据库。
$ sudo -u Wj createdb weizhu_zhongfu -O root -E UTF8 -e   # 手动创建名为weizhu_zhongfu的数据库。

```

安装redis
apt-get install redis-server


### 一些库

```
sudo apt-get install image imagemagick #图片处理库(不安装会导致图片上传处理有问题)
```


### vim配置

http://wiki.ruchee.com/public/_vimrc.html

### linux操作命令
```
#查看系统版本
uname -a  /  lsb_release -a
#curl 命令测试和学习http协议
curl -v http://z.cn
```
```
tail -f -n 20 log #显示log的后20行内容
```


### 常见错误警告
```
/home/wangjing/.rvm/gems/ruby-2.2.1/gems/railties-4.2.6/lib/rails/app_rails_loader.rb:39: warning: Insecure world writable dir /usr/local in PATH, mode 040777
/home/wangjing/.rvm/gems/ruby-2.2.1/gems/bundler-1.11.2/lib/bundler/shared_helpers.rb:78: warning: Insecure world writable dir /usr/local in PATH, mode 040777
解决：sudo chmod 775 /usr/local
该问题的原因是目录被赋予了777的权限，不安全。只要将目录权限修改为775，就可以解决。
```
```
/home/wangjing/.rvm/gems/ruby-2.2.1/gems/bundler-1.11.2/lib/bundler/runtime.rb:80:in `rescue in block (2 levels) in require': There was an error while trying to load the gem 'uglifier'. (Bundler::GemRequireError)
解决：sudo apt-get install nodejs
该问题的原因可能是uglifier是一个js的封装，缺少js的运行时环境或者js的解释器。安装一下就好了。
```
```
psql: could not connect to server: No such file or directory
	Is the server running locally and accepting
	connections on Unix domain socket "/tmp/.s.PGSQL.5432"?
解决：rm /usr/local/var/postgres/postmaster.pid
删除该文件即可。
```

### Ruby一些命令问题
```
You have already activated rake 11.1.2,but ... Gemfile ... 10.5.0 ...
问题是当前gem列表中最高版本是11.1.2，但是rails的Gemfile支持的最高版本是10.5.0，在rails无法升级的情况下，只能删掉11.1.2版本。
$ gem uninstall rake #然后选择要删的版本
```
```
$ gem list #显示gems版本
$ gem list rake #显示rake的版本
```

### 添加新用户（Ubuntu）
```
添加新用户包含：添加用户、目录、shell、权限等
1、添加新用户：useradd -r -m -s /bin/bash 用户名
2、配置新用户密码：passwd 用户名
3、添加root权限：vim /etc/sudoers   添加(用户名 ALL=(ALL:ALL) ALL)

sudo chown www www # 为用户添加目录权限
sudo chgrp www www # 为分组添加目录权限
```

### bashrc文件作用
```
/etc/profile:此文件为系统的每个用户设置环境信息,当用户第一次登录时,该文件被执行.
并从/etc/profile.d目录的配置文件中搜集shell的设置.
/etc/bashrc:为每一个运行bash shell的用户执行此文件.当bash shell被打开时,该文件被读取.
~/.bash_profile:每个用户都可使用该文件输入专用于自己使用的shell信息,当用户登录时,该文件仅仅执行一次!默认情况下,他设置一些环境变量,执行用户的.bashrc文件.
~/.bashrc:该文件包含专用于你的bash shell的bash信息,当登录时以及每次打开新的shell时该文件被读取.
~/.bash_logout:当每次退出系统(退出bash shell)时,执行该文件.
```
