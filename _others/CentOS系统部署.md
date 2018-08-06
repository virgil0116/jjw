## 项目部署

#### 阿里云服务器无法访问80端口

打开实例 -> 更多 -> 安全组配置 -> 配置规则 -> 添加安全组规则

```
端口范围：80/80
授权类型：地址段访问
授权对象：0.0.0.0/0
```

#### 安装软件(CentOS)

用root用户安装常用的服务，nginx、postgresql、redis-server、git、imagemagick、libpq-dev(centos-yum install postgresql-devel / postgresql96-devel)、nodejs

[nginx安装](http://blog.csdn.net/ysydao/article/details/51388385)

[postgresql安装](https://www.postgresql.org/download/linux/redhat/#yum)
[postgresql命令](http://www.cnblogs.com/lion382/p/4022597.html)
```
vim /var/lib/pgsql/data/pg_hba.conf
设置密码： 
su -postgres 
创建用户： shell中执行 createuser name
psql
\password name
alter role wikiflyer with superuser; # 设置权限
```
[redis安装](https://www.cnblogs.com/fanlinglong/p/6635828.html)
[redis教程](http://www.cnblogs.com/onephp/p/6245902.html)
[redis下载](https://redis.io/download)
[yum方式安装redis](https://www.cnblogs.com/autohome7390/p/6433956.html)

```
如果要安装最新的redis，需要安装Remi的软件源，官网地址：http://rpms.famillecollet.com/
```

```
1、用root用户安装常用的服务，nginx、postgresql、redis-server、git、imagemagick、libpq-dev、nodejs，用sudo 安装rvm，curl -sSL https://get.rvm.io | sudo bash -s stable ，然后安装ruby 2.3或2.4版本
2、adduser www(添加用户)，passwd www(修改密码) 然后修改/etc/sudoers 添加sudo权限
3、用www登录部署，可以发布在 /var/www 目录下，将这个目录的 chown chgrp 改成www用户
4、创建ssh key，将pub密钥作为deploy key
5、可以在home目录git clone代码，然后安装bundle，从这个目录用mina进行部署
$ gem install bundler
```
[免密码登录](http://chenlb.iteye.com/blog/211809)
```
.ssh文件夹权限是700，chmod 700 .ssh
authorized_keys权限是600，chmod 600 authorized_keys

$ ssh-keygen -t rsa -P ''
```


[nginx配置https](http://blog.csdn.net/weixin_35884835/article/details/52588157)

##### 使用ssl模块配置同时支持http和https并存
###### 一、生成证书
```
# 1、首先，进入你想创建证书和私钥的目录，例如：
cd /etc/nginx/

# 2、创建服务器私钥，命令会让你输入一个口令：
openssl genrsa -des3 -out server.key 1024
# 输入口令：lxyen

# 3、创建签名请求的证书（CSR）：
openssl req -new -key server.key -out server.csr

Country Name (2 letter code) [XX]:CN
State or Province Name (full name) []:ShanDong
Locality Name (eg, city) [Default City]:QingDao
Organization Name (eg, company) [Default Company Ltd]:lxyen
Organizational Unit Name (eg, section) []:lxyen
Common Name (eg, your name or your server's hostname) []:lxyen
Email Address []:lxyen@lxy.com

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:lxyen
An optional company name []:lxyen

# 4、在加载SSL支持的Nginx并使用上述私钥时除去必须的口令：
cp server.key server.key.org
openssl rsa -in server.key.org -out server.key


# 5、最后标记证书使用上述私钥和CSR：
openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt
```
###### 二，配置nginx
```
cd /etc/nginx
vim nginx.conf
#
# HTTPS server configuration
#
server {
    listen       443;
    server_name  本机的IP地址;

    ssl                  on;
    ssl_certificate      /etc/nginx/server.crt;
    ssl_certificate_key  /etc/nginx/server.key;

    ssl_session_timeout  5m;

#    ssl_protocols  SSLv2 SSLv3 TLSv1;
#    ssl_ciphers  ALL:!ADH:!EXPORT56:RC4+RSA:+HIGH:+MEDIUM:+LOW:+SSLv2:+EXP;
#    ssl_prefer_server_ciphers   on;

    location / {
        #root   html;
        #index  testssl.html index.html index.htm;
     proxy_redirect off;
     proxy_set_header Host $host;
     proxy_set_header X-Real-IP $remote_addr;
     proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
     proxy_pass http://IP地址/ssl/;
    }
}
```

### 后台开发

delete方法无效的解决方案：
```
logout delete方法：
admin.js 中引用//= require rails-ujs
在admin.html.erb中引用admin.js
使用：
<%= link_to admin_logout_path, class: 'dropdown-toggle', method: :delete, data: {confirm: '确定要注销吗？'}  do %>
<i class="icon-logout"></i>
<% end %>
```

### 插件
```
# 图片上传、剪切
gem 'carrierwave'
gem 'mini_magick'
# 备份组件
gem 'backup'
```

### HTTPS请求
[参考](https://www.wosign.com/news/2016-1111-01.htm)

***微信小程序要求HTTPS请求***为了保护小程序应用安全，微信官方的需求文档要求，每个微信小程序必须事先设置一个通讯域名，并通过HTTPS请求进行网络通信，不满足条件的域名和协议无法请求.

***SSL证书的选择***

1)SSL证书类型

* DV SSL证书(域名验证型)：只验证域名所有权，适合个人网站、博客等站点使用;
* IV SSL证书(个人验证型)：验证网站所属个人身份，适合自媒体、个人品牌站点使用;
* OV SSL证书(企业验证型)：验证网站所属单位身份，适合企业级用户使用;
* EV SSL证书(扩展验证型)：扩展验证网站所属单位身份，适合高度信任的企业级用户使用。




### 小程序

[微信小程序之下拉加载和上拉刷新](http://www.cnblogs.com/simba-lkj/p/6274232.html) \| [轮播](http://blog.csdn.net/u010635353/article/details/53158080) \| [小程序开发资源](http://www.henkuai.com/forum.php) \| [微信小程序联盟](http://www.wxapp-union.com/) \| 


### JQuery教程
[JQuery速查教程](http://jquery.cuishifeng.cn/)


#### Linux命令
```
sudo find / -name postgresql.conf # 查找文件postgresql.conf

```



















