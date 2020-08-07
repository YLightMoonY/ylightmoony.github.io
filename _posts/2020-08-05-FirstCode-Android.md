---
title: 第一行代码-Android 笔记
layout: post
---

# 第一行代码-Android



## Android启程

一些介绍就不废话了。

### Android 系统架构

1. Linux 内核层

   主体是驱动部分

2. 系统运行库

   为Android提供支持的库，AndroidRuntime等

3. 应用架构

   Android Framework，系统提供的API

4. 应用层

![Android 架构](./assets/FirstCode/Android-Framework.svg.png)



### Android 应用开发特色

这些是应用开发主要接触的内容，大部分是需要精通的，少部分至少有些了解

1. 四大组建

   Activity 所看到的页面

   Service 后台运行的服务

   Broadcast 广播

   ContentProvider 内容提供者，为其他应用提供访问本应用数据的路径

2. 丰富的系统控件

   系统本身提供大量的API以供第三方应用使用

3. SQLite数据库

   轻量级数据库，适合移动端使用

4. 多媒体

   支持音视频照相等等，一个综合个人智能终端

5. 定位

   基于网络，gsm，gps定位，系统API比较笨拙且因为不可说的原因不好用，大部分时候是使用第三方的诸如高德百度等sdk



### 开发环境

直接去 [Android中国官网](https://developer.android.google.cn/) 下载AndroidStudio即可

​	*ps: baidu到的可能是第三方网站，建议bing或者google*

安装的时候无脑下一步就行

### 新建项目 启动应用

略

### 项目结构

基于Gradle，代码在app\src下面，java中是java代码，res中是布局图片等资源文件。

现在新建的Activity都基于`androidx.appcompat.app.AppCompatActivity`类

** 之前版本的Support包中的内容都存于各个版本诸如v4 v7等，新版本全部都整合到了androidx下，虽然之前的还能用，建议使用新api**



## Activity （活动）
