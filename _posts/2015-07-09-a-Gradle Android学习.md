---
layout: post
title:  "Gradle Android 学习"
categories: Android
---
---
[开源项目](https://github.com/Trinea/android-open-project/tree/master/繁體中文版)
##Gradle Android
[Gradle学习](http://my.oschina.net/zjzhai/blog/220028)
[Gradle Android 中文翻译](http://avatarqing.github.io/Gradle-Plugin-User-Guide-Chinese-Verision/basic_project/configuring_the_structure.html)

[Android使用Gradle多渠道打包](http://blog.csdn.net/jjwwmlp456/article/details/45057067)

[Android多渠道打包(android-multi-channel-tool)](https://github.com/promeG/android-multi-channel-tool)

Gradle是任务驱动型构建工具，它的构建过程是基于Task的。

构建Android最基本的build.gradle文件：

```
buildscript{
	repositories{
		jcenter()
	}
	dependencies{
		classpath 'com.android.tools.build:gradle:1.2.3'
	}
}
apply plugin: 'android'
android{
	compileSdkVersion 19
	buildToolsVersion "19.0.0"
}
```
###通用任务
1. assemble 组装编译任务输出到output
2. check 运行所以检查(我的理解 检查编译错误)
3. build  执行assemnle和check
4. clean 清除构建的build文件

###3.基本项目
Android build file的3个主要部分：

1. buildscript{...}    
	* buildscript{...}这里配置驱动构建过程的代码。<br>
	* __注意：__这里的配置只影响控制构建过程的代码，不影响项目源代码。项目本身需要声明自己的仓库和依赖关系。
2. apply plugin
	* 项目类型
3. android{...}
	* 配置了所以android构建需要的参数。也是Android DSL的入口点。

如果目录不是按照默认约定的目录结构，需要在android{...}中的sourceSets{...}中配置一下。

__构建任务__(Build Task)

__android{...}的一搬格式__
---
```
android{
	compileSdkVersion 22
    buildToolsVersion "22.0.1"
    defaultConfig{
    	applicationId "包名"
    }
}
```

-----
android studio使用gradle打包，主要关注以下几点：

* 主要修改代码是android{...}
* 固定格式：compileSdkVersion/buildToolsVersion/defaultConfig{...}
* 修改针对：signingConfigs{...}/buildTypes{...}/productFlavors{...}
* signingConfigs{...}里面是单独的config(如：debug{...}/release{...}/myConfig{...}等)
* 其中buildTypes与productFlavors中的方法是交互打包的。
* 混肴文件在buildTypes的release中配置即可。


去掉重复依赖

```
compile 'com.alibaba.fastjson.latest.integration' {
    exclude module: 'annotations', group: 'com.google.android'
}
```

### 遇到问题
```
Error:Gradle version 2.10 is required. Current version is 2.2.1. If using the gradle wrapper, try editing the distributionUrl in /Users/Wj/Documents/git/daggerProject/EasyMVP/gradle/wrapper/gradle-wrapper.properties to gradle-2.10-all.zip
<a href="fixGradleVersionInWrapper">Fix Gradle wrapper and re-import project</a><br><a href="openGradleSettings">Gradle settings</a>

两种解决方案：
1. 通过设置本地下载最新的2.10版本的Gradle。
2. 默认Gradle，修改 项目/gradle/wrapper/gradle-wrapper.properties文件
distributionUrl=https\://services.gradle.org/distributions/gradle-2.2.1-all.zip 改为
distributionUrl=https\://services.gradle.org/distributions/gradle-2.10-all.zip
```



https://github.com/promeG/android-multi-channel-tool
http://blog.csdn.net/jjwwmlp456/article/details/45057067









