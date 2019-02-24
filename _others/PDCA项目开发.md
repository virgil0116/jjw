## PDCA项目

#### SSH命令行上传/下载文件
```
上传：scp /path/file（这部分为本地的路径） user（远端目标用户名）@host（远端目标IP）:/pathorfile（文件存储路径）
下载：scp user（远端用户名）@host（远端IP）:/path/file（下载文件在远端的路径） localpathorfile（本地文件存放路径）

scp www@139.199.153.232:/data/backup/pdca_backend/pdca-2017-07-311200.backup ./backup/pdca.backup
# scp ./backup/pdca.backup dream@42.121.108.45:
```
#### 定时任务自动备份

[Gem-whenever](https://github.com/javan/whenever) \| [参考whenever实现周期性任务](http://www.jianshu.com/p/8046ed8fb5ae?utm_campaign=hugo&utm_medium=reader_share&utm_content=note&utm_source=weixin-friends) \| [参考PostgreSQL数据备份与还原](http://www.jianshu.com/p/c653b3284504?utm_campaign=hugo&utm_medium=reader_share&utm_content=note&utm_source=weixin-friends)

* deploy.rb中添加config/schema.rb文件
* 编辑文件内容(测试服务器)

```
set :environment, :production

every 1.days do
     command "pg_dump -U root -f /data/backup/pdca_backend/pdca-`date --date='0 days ago' +%Y%m%d%H%M%S`.backup pdca_backend_pro"
     # command 'pg_dump -U jing -f pdca_backend_dev.out pdca_backend_dev'
end
```
* RAILS_ENV=production bundle exec whenever -w # 执行定时任务
* crontab -l # 检查定时任务

#### 备份数据库数据
[PostgresSQL pg_dump&psql数据的备份和回复](http://blog.csdn.net/luojinbai/article/details/43700265)

* 纯文本格式的脚本备份和恢复
* 归档文件格式备份和恢复
* 压缩备份和恢复

```
# 备份线上数据库
pg_dump -U www -F t -f /data/backup/pdca_backend/pdca-`date +\%F\%H\%M`.tar pdca_backend_pro
# 恢复线上数据库
pg_restore -U root -d pdca_stage ./backup/pdca.tar

pg_dump -U root -f /vendemo.tar vendemo
pg_restore -U root -d vendemo1 /vendemo.tar
```
#### 版本发布
```
分支：master、dev、v2
master是正式版的分支，发布位置：预发版服务器和正式服务器；
dev/v2是两个开发分支；
dev是开发master问题处理分支；
v2是新功能开发分支；
```
#### rsync命令：远程数据同步工具
[参考](http://man.linuxde.net/rsync)

```
```
#### 测试服务器从正式服务器pull数据
```
1、将正式服务器的备份数据拉取过来
scp www@139.199.153.232:/data/backup/pdca_backend/pdca-2017-08-021200.tar ./pdca.tar
2、使用上一步导出的xxxx.tar本分文件恢复数据库
pg_restore ./pdca.tar  -c -U root -d pdca_stage -O
pg_restore -O -U apple -d pdca_backend_dev ./backup/pdca.tar
3、下载正式服务器上的图片、文件等资源
scp -r www@139.199.153.232:/var/www/pdca/shared/public/uploads/* ./shared/public/uploads/
4、通过rsync同步命令同步正式服务器上的图片、文件等资源（增量更新）
rsync -avz www@139.199.153.232:/var/www/pdca/shared/public/uploads/* ./shared/public/uploads/
rsync -avz www@139.199.153.232:/var/www/pdca/shared/public/uploads/* ./shared/public/uploads/ -v -r
```

#### Gem使用
[ancestry](https://github.com/stefankroes/ancestry)

```
在使用的model中添加两个字段：ancestry(任务树形结构)和ancestry_depth(深度缓存)两个字段

```

### Linux系统技术

[linux 数据盘分区并挂载](https://blog.csdn.net/ljihe/article/details/52293724)


### 前端细节

[Element文档](http://element.eleme.io/#/zh-CN)

[vux文档](https://doc.vux.li/zh-CN/)

[元素吸顶或固定位置](https://blog.csdn.net/wang1006008051/article/details/78003974)

[元素滚动到某个位置](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/scrollIntoView)


















