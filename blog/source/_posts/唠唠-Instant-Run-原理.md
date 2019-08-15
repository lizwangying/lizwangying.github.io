---
title: 唠唠 Instant Run 原理
date: 2019-04-20 16:39:44
tags: 操作字节码
---
#### 本文参考链接：
[Instant Run 官方源码与说明](https://android.googlesource.com/platform/tools/base/+/studio-master-dev/instant-run/)  
[Instant Run Google Developer 博客](https://medium.com/google-developers/instant-run-how-does-it-work-294a1633367f)  
[Android Studio 官方源码以及说明](https://android.googlesource.com/platform/tools/base/+/studio-master-dev/source.md)

#### 本文大纲：
1. 什么是 `instant run`
2. 增量编译过程   
	a. HotSwap  
	b. WarmSwap  
	c. ColdSwap  
3. 分为多个`slice_x-classes.dex`包，每个包的内容和信息
4. 源码解析
5. 和插件化的关系
6. Instant Run 怎么把 `Application` 运行起来的

#### 本文涉及知识点
* Transform
* JVM ，classloader
* Gradle


## 什么是 Instant Run 
Android studio 提供的增量编译安卓应用的工具，开发过大型项目的同学都知道，如果每次运行 APP 都是从 0 开是编译，一天要浪费很长时间，有了 `instant run` 则能够增量编译，极大节省时间。

## 增量编译过程
