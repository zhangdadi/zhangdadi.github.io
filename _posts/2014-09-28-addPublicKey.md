---
layout: post
title: ssh添加公钥
categories: [blog ]
tags: [git ]
description: 
---

## 生成本地ssh密钥

1. 先查看自己本地是否有ssh密钥，键入命令："ls ~/.ssh"，如果本地已有ssh公钥会显示出id_rsa等文件，如下图：

   ![icon](http://zhangdadi.github.io/image/git/ssh/1.png)

2. 如果本地没有ssh公钥则生成ssh密钥，键入命令："ssh-keygen"，然后按三次“回车键”，不用输入密码，成功生成后如下图：
   
   ![icon](http://zhangdadi.github.io/image/git/ssh/2.png)

3. 用vim打开公钥，键入命令："vim ~/.ssh/id_rsa.pub
"，然后复制公钥的内容，公钥内容如下图：
![icon](http://zhangdadi.github.io/image/git/ssh/3.png)

4. 先按“Esc键”后键入命令：“:q”，按下"回车键"退出vim，如下图
![icon](http://zhangdadi.github.io/image/git/ssh/4.png)

## 添加公钥到git服务器

1. 键入命令"ssh <用户名>@<服务器地址>"，提示输入密码时则输入密码，成功进入git服务器后如下图所示：
![icon](http://zhangdadi.github.io/image/git/ssh/5.png)

2. 键入命令"mkdir .ssh"，新键一个.ssh目录
3. 键入命令"vim .ssh/authorized_keys"，打开authorized_keys文件，然后按“I键”，让vim进入插入状态，再把刚刚复制下来的本地公钥内容粘贴到此文件内，如下图所示：![icon](http://zhangdadi.github.io/image/git/ssh/6.png)
4. 先按“Esc键”后键入命令：“:q”，按下"回车键"退出vim，如下图所示：![icon](http://zhangdadi.github.io/image/git/ssh/7.png)
5. 再重新打开命令行，键入命令"ssh <用户名>@<服务器地址>"，如果不用输入密码就成功进入git服务器，则已成功添加公钥，若不成功请重新严格按步骤执行。


##版本变更

| 版本 | 日期 | 修订人 | 修改内容 |
| ----|-------|--------|:---------:| 
| 1.0 | 2014-6-20 | 张达棣 | 初版 |