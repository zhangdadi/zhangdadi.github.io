---
layout: post
title: NSString为什么要用copy修饰呢
categories: [blog ]
tags: [iOS ]
description: 
---

![1](http://zhangdadi.github.io/image/copy/1.png)

从上图中可以看到，类DemoStr的strongDemo和copDemo的内存

![1](http://zhangdadi.github.io/image/copy/2.png)

通过对比可以看到：使用的copy后，如果源数据是可变字符串却会深拷贝，而源数据是不可变字符串则进行国拷贝。
