---
layout: post
title:  "jekyll -> github"
date:   2016-03-03
categories: 其他
---

[官方文档](http://jekyll.bootcss.com/docs/home/)

代码推上了github，打开一看，排版直接让我奔溃。

噢，我的天呐，这太神奇了！我都不知道我错在哪了。不过，最终还是让我找到了问题(O(∩_∩)O哈哈~)，直到我看了[这位仁兄](http://brianway.github.io/2016/01/07/build-blog-with-jekyll-and-github/)说得。

我的问题是仓库名错误，应该建一个这样的 **username.github.io** 的仓库，而我建了一个这样的 **oldetan** 仓库。这是不对的，具体原因等我查查再分析一下(但愿我还记得)。

搭建 **jekyll -> github** 的步骤：

* 建立一个这样的 **username.github.io** 的仓库
* 本地能够运行的jekyll环境(以便本地查看效果)
* 将本地的调试成功的代码 ***push*** 到 **username.github.io** 仓库中(等几分钟即可看到效果)。

本地jekyll搭建

```
安装ruby、gem(Ruby的包管理工具)
安装jekyll ： gem install jekyll
安装rdiscount ： gem install rdiscount //用来解析Markdown标记的解析包。
写markdown
jekyll server 启动博客

```

## markdown添加图片方法

```
(https://raw.githubusercontent.com/username/respository/master/resources/Dict.png =100x)
这是图片的地址
username 		github用户名
respository  	github上的仓库名称
resources		存放图片的文件夹名

markdown使用方法
 ![dict icon](https://raw.githubusercontent.com/easyer/easyer.github.io/master/resources/Dict.png =300x)

300x 			图片的缩放比例
```

OVER！
