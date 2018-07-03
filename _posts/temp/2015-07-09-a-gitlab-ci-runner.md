---
layout: post
title:  "持续集成 GitLab-CI"
categories: 技术
---
---
###持续集成
是一种软件开发实践，软对开发人员要经常集成他们的代码，每天会集成n次。每次集成都通过自动化(编译、打包、发布、自动化测试)来验证，从而可以尽早的发现问题，解决问题。

__优点__<br>
1. 减少风险 每次集成都会及时做相应的自动化测试。
2. 减少重复的过程 将代码编译、数据库集成、测试、审查、打包、部署及反馈做成自动化的过程，为开发人员减少了很多不必要的重复劳动时间。
3. 任何时间和地点生成可部署的软件 任何时间和地点都可以对代码进行修改并提交到代码库中，完成自动化的集成。
4. 增强项目的可见性 每次对代码的改动都会实时的更新到项目的部署环境，能看到真实的改动情况。
5. 建立团队对开发产品的信心 清楚的知道每一次代码改动对产品的影响，和产生的结果。

###持续集成环境
持续集成的环境都多种，目前公司打算使用的是gitlab + gitlab ci + runner的环境。


###Reigster Runner
https://github.com/ayufan/gitlab-ci-multi-runner/blob/master/docs/install/osx.md

###配置.gitlab.yml文件
用来构建Runner，该文件在项目的跟目录下。

[配置文档](http://doc.gitlab.com/ci/builds_configuration/README.html)

`build:
  script:
     - mkdir wjjjjjj
  only:
    - master`	

###Runner配置文件
/Users/Wj/Library/LaunchAgents/gitlab-ci-multi-runner.plist

###Build script example
https://gitlab.com/gitlab-org/gitlab-ci/tree/master/doc/examples

##GitLab Documentation
###用.gitlab-ci.yml构建你的配置
从7.12版本，GitLab CI使用.gitlab-ci.yml文件来构建你的配置。它在你仓库的根目录并且包含三种类型的对象:before_script,builds和deploy_builds.

(代码见网页)

让我们仔细看看每一个部分。

__builds__

这里你可以指定构建的参数：

(代码见网页)

rspec是这个对象的一个key，并且它指定你构建的名字。
script是一个runner执行的shell脚本。它也可以由before_script准备。这个参数也可以包含多个命令行通过数组:

`script:
	- uname -a
	- bundle exec rspec`
	
你可以在[refs settings explanation](http://doc.gitlab.com/ci/builds_configuration/README.html#refs-settings-explanation)中读到only和except参数.

__deploy_builds__

部署构建将会被运行当所有其他的构建都成功了。使用简单的语法定义：

(代码见网页)

__before_script__

before_script是用来定义命令在所有的构建(包括部署构建)之前。可以是一个数组或一个多行字符串。

__参数设置解释__

有两个参数帮你设置在CI上构建和部署的策略。

* only:	   - master  定义准确的branch或tag的名字，将会被运行。也可以定义为正则格式。
* only:    - /^issue-.*$/
* except:  - "deploy"  这个参数是用来排除一些引用。也可以定义为正则格式。
* except:  -  branches  也可以表示特殊值像branches或tags。意思是用来排除所有的tags和branches。

__用.gitlab-ci.yml调试你的构建__

每一个CI实例都有一个嵌入的调试工具。

基本格式:

```
before_script:
	- command
	- command
 script01:
 	script:
 		- command
 		- command
 	tags:
 		- ruby
 		- android
 	only:
 		- master
 script02:
 	script:
 		- deploy command
 		- command
 	type: deploy
 	tags:
 		- ruby
 		- android
 	except:
 		- stable
```













