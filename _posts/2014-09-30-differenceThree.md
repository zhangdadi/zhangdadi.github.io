---
layout: post
title: delegate,block,notification三者区别？
categories: [blog ]
tags: [iOS ]
description: 
---

我们经常在开发中遇到各种回调的时候，delegate,block,以及通知以上三者经常用到，这3者有什么区别呢，客官请听我慢慢道来。

## notification
“一对多”，在APP中，很多控制器都需要知道一个事件，应该用通知；但是通知用多的将会很恶心，可以用delegate的多点回调代替部分通知。

## delegate
* “一对一”，对同一个协议，一个对象只能设置一个代理delegate，所以单例对象就不能用代理（可以用多点回调，下面见解）。
* 代理更注重过程信息的传输：比如发起一个网络请求，可能想要知道此时请求是否已经开始、是否收到了数据、数据是否已经接受完成、数据接收失败。

## block
* 写法更简练，不需要写protocol、函数等等。
* block注重结果的传输：比如对于一个事件，只想知道成功或者失败，并不需要知道进行了多少或者额外的一些信息。
* block需要注意防止循环引用。

```
ARC下这样防止：
__weak typeof(self) weakSelf = self;
  [yourBlock:^(NSArray *repeatedArray, NSArray *incompleteArray) {
       [weakSelf doSomething];
    }];

非ARC
__block typeof(self) weakSelf = self;
  [yourBlock:^(NSArray *repeatedArray, NSArray *incompleteArray) {
       [weakSelf doSomething];
    }];
```

## delegate的多点回调
delegate的多点回调相对notification更加便捷，更多方便，让项目更好维护。
demo传送门：[点击我](https://github.com/zhangdadi/HDMultiPointCallNode)

下面是多点回调类的代码：

HDMultiPointCallNode.h

```
/**
 *  不retain计数的多点回调
 */
@interface HDMultiPointCallNode : NSObject

/**
 *  加入委托对象
 *
 *  @param data delegate对象
 */
- (void)pushData:(id)data;

/**
 *  把delegate从回调队列里移除
 *
 *  @param index 索引
 */
- (void)popIndex:(NSInteger)index;

/**
 *  把delegate从回调队列里移除
 *
 *  @param data 对象实例
 */
- (void)popData:(id)data;

/**
 *  多点回调
 *
 *  @param block 需要回调的delegate
 */
- (void)callMultiPoint:(void (^)(id data))block;
```

HDMultiPointCallNode.m

```
@interface HDMultiPointCallNode ()
@property (nonatomic, weak) id data;
@property (nonatomic, strong) HDMultiPointCallNode *next;

@end

@implementation HDMultiPointCallNode

- (void)pushData:(id)data {
    [self popData:data];
    HDMultiPointCallNode *head = self;
    while (head.next != nil) {
        head = head.next;
    }
    
    HDMultiPointCallNode *newNode = [[HDMultiPointCallNode alloc] init];
    newNode.data                  = data;
    head.next                     = newNode;
}

- (void)popData:(id)data {
    HDMultiPointCallNode *head = self;
    HDMultiPointCallNode *temp = self.next;
    
    while (head.next != nil) {
        if (data == temp.data) {
            head.next = temp.next;
            temp.data = nil;
            temp      = nil;
        }
        head = head.next;
        temp = temp.next;
    }
}

- (void)popIndex:(NSInteger)index {
    HDMultiPointCallNode *head = self;
    HDMultiPointCallNode *temp = self.next;
    
    for (int i = 0; head.next != nil && i <= index; ++i, head = head.next, temp = temp.next) {
        if (i == index) {
            head.next = temp.next;
            temp.data = nil;
            temp      = nil;
        }
    }
}

- (void)callMultiPoint:(void (^)(id data))block {
    HDMultiPointCallNode *head = self;
    
    for (int i = 0; head.next != nil; ++i) {
        head = head.next;
        if (head.data != nil) {
            block(head.data);
        } else {
            [self popIndex:i];
        }
    }
}

@end

```

重写delegate的set方法，在set方法里调用pushData，如下：

```
- (void)setDelegate:(id<HDMessageManageDelegate>)delegate {
    [_callNode pushData:delegate];
}
```
在需要回调的地方如下调用方式：

```
//图片下载完成，回调通知
            [_callNode callMultiPoint:^(id data) {
                [self callNewMessageDelegate:data obj:obj msgKing:msgKing];
            }];
```            