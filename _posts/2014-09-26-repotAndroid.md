---
layout: post
title: cocos2dx-mac下 cocos2dx 移植到android平台
categories: [blog ]
tags: [2dx, iOS ]
description: 
---

今天成功完成在MAC下移植cocos2dx代码到android平台,故在此把过程记录下来,希望能帮到有缘人.

### 在mac下搭建起Eclipse的Android环境

* eclipse的传送门:[点击我](http://eclipse.org/downloads/),我下载的eclipse版本是Eclipse IDE for Java Developers安装好后打开eclipse
* 进入菜单中的 "帮助" -> "安装新软件", 
* 点击添加...按钮，弹出对话框要求输入名称和位置：名称自己随便取，位置输入:http://dl-ssl.google.com/android/eclipse
* 确定后，在work with后的下拉列表中选择刚才添加的ADT，会看到下面出有Developer Tools，展开它会有Android DDMS和Android Development Tool，勾选他们,之后就是一直下一步的,不用多说的.

### 安装NDK 
NDK下载下来直接解压就可以的, NDK传送门:[点击我](http://www.cnblogs.com/zhangdadi/admin/%20%20http:/developer.android.com/sdk/ndk/index.html)

### 配置cocos2d-x编译路径(重点哦)
我的相关路径如下:

* cocos2d-x :/Users/zdadi/cocos2d-2.0-x-2.0.4
* android SDK:/Users/zdadi/android-sdks
* android NDK: /Users/zdadi/android-ndk-r8d 

开始配置路径 打开终端后输入以下命令显示隐藏文件

```
defaults write com.apple.finder AppleShowAllFiles -bool True  
killall Finder 
 
```
然后打开MAC的finder 进到用户根目录下用文本编辑器打开.bash_profile文件 再手动在文件中输入如下路径:

```
export ANDROID_SDK_ROOT=/Users/zdadi/android-sdks                               
export ANDROID_NDK_ROOT=/Users/zdadi/android-ndk-r8d                            
export COCOS2DX_ROOT=/Users/zdadi/cocos2d-2.0-x-2.0.4              
export NDK_ROOT=$ANDROID_NDK_ROOT                                
export PATH=$PATH:$ANDROID_SDK_ROOT  
export PATH=$PATH:$ANDROID_NDK_ROOT  
```

如下图:![1](http://zhangdadi.github.io/image/coco2dx/06.png)

`路径最好不要有空格！` 当然你也可以vim之类的工具打开

### 现在来编译自带的例子

打开cocos2dx源代码里的cocos2dx/platform/android/java/src/org/cocos2dx目录   如我的目在:/Users/zdadi/cocos2d-2.0-x-2.0.4/cocos2dx/platform/android/java/src/org/cocos2dx
       然后拷贝此目录上的lib文件夹到cocos2dx源代码里的samples/HelloCpp/proj.android/src/org/cocos2dx目录下   如我的位置在:  /Users/zdadi/cocos2d-2.0-x-2.0.4/samples/HelloCpp/proj.android/src/org/cocos2dx/lib 
以后新建android项目都要拷贝一次,是不是觉得太麻烦,不用怕,下面有解决方法
打开终端

* 输入:　　cd $COCOS2DX_ROOT/samples/HelloCpp/proj.android　　回车
* ./build_native.sh　　回车

正常情况会进入编译,好下图![1](http://zhangdadi.github.io/image/coco2dx/07.png)

### 在android真机上跑起来 (模拟器不支持OpenGL 2.0)
编译成功后，打开Eclipse 导入刚刚的HellpCpp 过程如下图,不多说的,
然后连上真机编译运行,如下图
![1](http://zhangdadi.github.io/image/coco2dx/08.png)

### 自己手动建项目

* 创建项目之前请先打开cocos2dx源代码里的template/android/copy_files.sh文件 ,我的文件路径:/Users/zdadi/cocos2d-2.0-x-2.0.4/template/android/copy_files.sh 
       在copy_files.sh让你说的里找到 copy_src_and_jni() {...},在里面添加  cp -rf $COCOSJAVALIB_ROOT/src $APP_DIR/proj.android 如下图 
       
       ![1](http://zhangdadi.github.io/image/coco2dx/09.png)
       
       这样就不用每次都执行第4步的拷贝操作的
       
* 再打开cocos2dx源代码里的template/android/gamemk.sh文件, 我的文件路径:/Users/zdadi/cocos2d-2.0-x-2.0.4/template/android/gamemk.sh 
* 在文件下面 找到  LOCAL_C_INCLUDES 项 将其修改为以下代码: 

```
LOCAL_C_INCLUDES := \$(LOCAL_PATH)/http://www.cnblogs.com/Classes \\  
  
                    \$COCOS2D_ROOT/cocos2dx \\  
  
                    \$COCOS2D_ROOT/cocos2dx/platform \\  
  
                    \$COCOS2D_ROOT/cocos2dx/include \\  
  
                    \$COCOS2D_ROOT/CocosDenshion/include  
 ```
 
 如下图:
 ![1](http://zhangdadi.github.io/image/coco2dx/10.png)
 
 完成第一第二步之后,以后你建的cocos2dx项目就不用再放在cocos2dx源代码下的,移动到哪都可以,也不用每次创建新的android项目又重新配置一次Android.mk和build_native.sh, 当然你自己新建的其它类要配置下android.mk文件的
 
### 打开终端

* 输入:　　cd $COCOS2DX_ROOT回车 (进入到cocos2dx源代码目录)
* 输入:./create-android-project.sh  回车(创建新的android项目)

然后依次按提示输入创建android项目所需的参数
创好android项目后,其目录文件夹情况如下图
 ![1](http://zhangdadi.github.io/image/coco2dx/11.png)
 
 只要把建好的android项目目录下的proj.android文件夹   拷贝到  你用xcode建好的cocos2dx项目 和 里面的 IOS文件夹放在同级目录下就可以的,比如我用xcode创建的cocos2dx项目也是Hello 则我把proj.android拉到的位置:/Users/zdadi/Documents/代码/2DX/Hello/Hello/proj.android 
如右图: 
![1](http://zhangdadi.github.io/image/coco2dx/12.png)

以后想交叉编译到android平台就重复上面第4步"现在来编译自带的例子" 至于拷贝操作就不用的.