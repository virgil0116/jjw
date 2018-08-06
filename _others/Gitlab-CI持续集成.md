## CI集成测试

* 创建一个项目
* 配置测试和CI

#### 配置简单.gitlab-ci.yml
[配置.gitlab-ci.yml的工作](https://docs.gitlab.com/ce/ci/yaml/README.html)

添加.gitlab-ci.yml文件在项目根目录，配置项目的Runner，然后项目的每次提交推送都会触发CI的管道(pipeline)。

.gitlab-ci.yml告诉gitlab runner该做什么。默认情况下，它运行三个阶段的管道：构建build、测试test和部署deploy。不需要完全按照这三个阶段执行，没有的可以忽略。

使用持续交付和持续部署可以自动的将代码部署到测试或者正式的服务器上。

一个简单的CI操作步骤可总结为：

1. 添加.gitlab-ci.yml到项目的根目录。
2. 配置Runner。

配置好以后，项目的每次提交Runner都会自动启动管道，并且运行的管道会显示在项目的管道页面中。

条件：

1. Gitlab的版本使用8.0+r以上的版本。
2. 在Gitlab上有一个使用CI的项目。

.gitlab-ci.yml在项目根目录，而且在版本控制下的。因此老的版本可以成功构建，不同的分支也可以成功构建。

```yml
注意：.gitlab-ci.yml是yaml文件，需要按照yaml文件的格式来编写。
```

```ruby
before_script:     # 每一项工作执行前都会执行
  - apt-get update -qq && apt-get install -y -qq sqlite3 libsqlite3-dev nodejs
  - ruby -v
  - which ruby
  - gem install bundler --no-ri --no-rdoc
  - bundle install --jobs $(nproc)  "${FLAGS[@]}"

stages:		# 定义执行的阶段（过程，依次执行，前一个执行完才会执行下一个）
  - build
  - test
  - deploy

job1:
  stage: build
  script:
    - pwd
  only:		# 执行哪些分支可以执行
    - master
  except:		# 除了哪些标签,其他可以执行
    - dev
  tags:		# 指定哪些标签可以执行
    - docker
  allow_failure: true		# 允许出错，即出错以后，ci也能跑通

rspec:
  script:
    - bundle exec rspec

rubocop:
  script:
    - bundle exec rubocop
after_script:		# 每一项工作执行后都会执行
  - pwd

```
这是个简单配置能够运行大多数的Ruby程序。

* 定义了两项工作，rspec和rubocop执行不同的命令，名称是可以更改的。
* 在每项工作前都执行before_script中的命令，跟测试很像。

可以通过gitlab提供的ci/lint工具来检查编写的.gitlab-ci.yml的有效性。


#### 配置Runner
[安装Runner](https://docs.gitlab.com/runner/install/linux-repository.html) |
[注册Runner](https://docs.gitlab.com/runner/register/index.html) |
[安装postgres](https://www.postgresql.org/download/linux/redhat/#yum)

在Gitlab中，Runner负责运行.gitlab-ci.yml定义的工作。Runner可以是一台虚拟机、VPS、裸机、docker容器或者一组容器。Gitlab和Runner通过api交互，所以，唯一的要求就是Runner是连接互联网的。

Runner可以服务于特定的项目或者多个项目，如果服务于多个项目就被称为是Shared Runner.

在项目的Settings->CI/CD中查看Runner。

***安装Runner***

```
1、添加源
# For Debian/Ubuntu/Mint
curl -L https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh | sudo bash

# For RHEL/CentOS/Fedora
curl -L https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.rpm.sh | sudo bash

2、安装
# For Debian/Ubuntu/Mint
sudo apt-get install gitlab-runner

# For RHEL/CentOS/Fedora
sudo yum install gitlab-runner

3、安装指定的版本
# for DEB based systems
apt-cache madison gitlab-runner
sudo apt-get install gitlab-runner=10.0.0

# for RPM based systems
yum list gitlab-runner --showduplicates | sort -r
sudo yum install gitlab-runner-10.0.0-1

4、注册Runner
$ gitlab-runner register
Please enter the gitlab-ci coordinator URL (e.g. https://gitlab.com )
https://gitlab.wikiflyer.cn/

Please enter the gitlab-ci description for this runner
[hostame] my-runner

Please enter the gitlab-ci tags for this runner (comma separated):
my-tag,another-tag

Whether to run untagged jobs [true/false]:
[false]: true

Whether to lock Runner to current project [true/false]:
[true]: true

Please enter the executor: ssh, docker+machine, docker-ssh+machine, kubernetes, docker, parallels, virtualbox, docker-ssh, shell:
docker
这里选择的是shell

success!

```
#### 注册完Gitlab-Runner的配置

修改登录用户

```
$ sudo vim /usr/share/gitlab-runner/post-install
# detect user: first try to use gitlab_ci_multi_runner
for USER in xxxx gitlab_ci_multi_runner gitlab-runner; do
  if id -u "$USER" >/dev/null 2>/dev/null; then
    echo "GitLab Runner: detected user $USER"
    break
  fi
done

注意：xxx为期望登录的用户。
然后在/usr/share/gitlab-runner/目录下新建builds目录。
$ sudo chgrp xxx builds/			# 修改目录所属组
$ sudo chown xxx builds/			# 修改目录所属用户

```
配置完成！

### 单元测试

```ruby
gem 'ffaker', '~> 2.7'						# 生成模拟数据
gem 'rspec-rails', '~> 3.7'
gem 'airborne'								# rspec驱动api测试
gem 'shoulda-matchers', '~> 3.1'			# 断言验证
gem 'database_cleaner' 						# 清除测试数据
gem 'simplecov', :require => false, :group => :test   # 测试覆盖率

```

### 功能测试

```ruby
gem 'capybara', '~> 2.15.2'		# 功能测试框架
gem 'launchy', '~> 2.4.3'			# 自动打开调试帮助
gem 'poltergeist'					# 无界面驱动测试
```























































