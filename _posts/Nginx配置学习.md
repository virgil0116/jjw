## Nginx配置学习

[nginx官网](http://nginx.org/en/linux_packages.html)

#### Centos下安装nginx

* cd /etc/yum.repos.d/
* sudo vim nginx.repo
* 填写以下内容

```
[nginx]
name=nginx repo
baseurl=http://nginx.org/packages/centos/7/$basearch/
gpgcheck=0
enabled=1
```

* yum install nginx -y   # 安装命令
* sudo systemctl start nginx.service   # 启动nginx
* sudo systemctl restart nginx.service   # 重启nginx
* sudo systemctl stop nginx.service   # 停止nginx
* 如果无法访问需配置下防火墙

```
$ iptables -I INPUT 5 -i eth0 -p tcp --dport 80 -m state --state NEW,ESTABLISHED -j ACCEPT
$ service iptables save
$ service iptables restart
```
* 也可以用如下命令启动nginx

```
service nginx start # 启动Nginx服务
service nginx stop # 停止Nginx服务
service nginx.conf # Nginx配置文件位置
```
