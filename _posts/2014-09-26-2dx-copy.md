---
layout: post
title: 书店的灯光
categories: [blog ]
tags: [book, life, travel]
description: 每一个书店 都有自己的面容与灵魂。
---



* 在D盘根目录上新建一个文件夹cocos2dxSource，再把D:\cocos2d-2.0-x-2.0.4目录下的cocos2dx,CocosDenshion和external拷贝到这个目录下面，并且再新建一个文件夹libs，具体目录结构如下图：
![1](http://zhangdadi.github.io/image/coco2dx/1.png)
* 把D:\cocos2d-2.0-x-2.0.4\Release.win32目录下的七个lib文件拷贝到D:\cocos2dxSource\libs中，如下图：
![1](http://zhangdadi.github.io/image/coco2dx/2.png)
* 设置VC的头文件包含目录和库引用目录：选择"属性管理器",然后选择Debug | Win32，如下图：
![1](http://zhangdadi.github.io/image/coco2dx/3.png)
* 双击打开Microsoft.Cpp.Win32.user这个文件，然后选择VC++目录，如下图：
* 分别在"包含目录"和"库目录"的右边的输入框处点击一下，如下图：
![1](http://zhangdadi.github.io/image/coco2dx/4.png)
* 点编辑，把下图所示的目录都添加进去

    ```
D:\cocos2dxSource\cocos2dx\platform
D:\cocos2dxSource\cocos2dx
D:\cocos2dxSource
D:\cocos2dxSource\cocos2dx\include
D:\cocos2dxSource\cocos2dx\platform\third_party\win32\OGLES
D:\cocos2dxSource\CocosDenshion\include
D:\cocos2dxSource\cocos2dx\kazmath\include
D:\cocos2dxSource\cocos2dx\platform\win32
D:\cocos2dxSource\external
```
* 最后一步。在链接器->常规->附加库目录里添加 D:\cocos2dxSource\libs，如下图：
![1](http://zhangdadi.github.io/image/coco2dx/1.png)
* 如果cocos2d-x升级了，重新生成lib和dll，然后覆盖之前的就行了。接着再拷贝cocos2dx,CocosDenshion和external三个文件夹，也是覆盖就OK！
