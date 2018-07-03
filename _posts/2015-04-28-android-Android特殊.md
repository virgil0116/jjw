---
layout: post
title:  "Android问题"
date:   2015-04-28
categories: Android
---
---

values-v11 values-v14两个文件夹的作用
---

* values-v11代表在API 11+的设备上，用该目录下的styles.xml代替res/values/styles.xml
* values-v14代表在API 14+的设备上，用该目录下的styles.xml代替res/values/styles.xml

MimeType的用途以及所有类型
---
Android中MimeType的用途：Intent-Filter中的<data>有一个mimeType，它的作用是告诉Android系统本Activity可以处理的文件的类型。如设置为“text/plain”表示可以处理“.txt”文件。<br>
MimeTypeMap类是专门处理mimeType的类。

Android中组件焦点抢占问题
---
在Android中经常出现焦点抢占的问题，有时候有多个组件抢占焦点使程序不能实现预想的效果。解决这个问题可以在父控件中添加如下属性来控制：
android:descendantFocusability
* android:descendantFocusability="blockDescendants"//父控件会组织它的子控件获取焦点
* android:descendantFocusability="beforeDescendants"//父控件会在它的子控件之前获取焦点
* android:descendantFocusability="afterDescendants"//当控件没有子控件获取焦点的情况下获取焦点

TextView相关
---
在给TextView添加OnClick事件的时候，还需要配置android:clickable="true",否则点击无效。

Android传感器(Sensor)
---
1. define SENSOR_TYPE_ACCELEROMETER        //加速度
2. define SENSOR_TYPE_MAGNETIC_FIELD       //磁力
3. define SENSOR_TYPE_ORIENTATION          //方向
4. define SENSOR_TYPE_GYROSCOPE            //陀螺仪
5. define SENSOR_TYPE_LIGHT                //光线感应
6. define SENSOR_TYPE_PRESSURE             //压力
7. define SENSOR_TYPE_TEMPERATURE          //温度
8. define SENSOR_TYPE_PROXIMITY            //接近
9. define SENSOR_TYPE_GRAVITY              //重力
10. define SENSOR_TYPE_LINEAR_ACCELERATION //线性加速度
11. define SENSOR_TYPE_ROTATION_VECTOR     //旋转矢量<br>
[参考](http://www.cnblogs.com/tyjsjl/p/3695808.html)

Android Studio错误解决
---
```
“Error:(1) Attribute "title" has already been defined”
“Error:(1) Attribute "dividerPadding" has already been defined”
“Error:(1) Attribute "textAllCaps" has already been defined”
```
类似于这种情况的错误可能是v4和v7包冲突导致的。
禁止弹出软键盘
---
1. 在AndroidManifest.xml中选择对应的activity，将windowSoftInputMode属性设置为adjustUnspecified|stateHidden(__测试不好用__)
2. 让EditText失去焦点，editText.clearFocus();(__暂时未测__)
3. 强制隐藏输入法：InputMethodManager imm = (InputMethodManager)getSystemService(Context.INPUT_METHOD_SERVICE);immHideSoftInputFromWindow(editText.getWindowToken(),0);(__暂时未测__)
4. EditText始终不弹软键盘：editText.setInputType(InputType.TYPE_NULL);(__测试可用__)
5. [参考](http://kfc-davy.iteye.com/blog/1098403)

点击EditText之外隐藏键盘
---
http://blog.csdn.net/mad1989/article/details/25069821
