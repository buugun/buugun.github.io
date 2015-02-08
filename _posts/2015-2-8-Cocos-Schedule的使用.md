---
layout: post
title: "Cocos-Schedule的使用"
description: "Cocos-Schedule的使用"
keywords: "cocos, schedule"
category: Cocos
tags: [Cocos, schedule]
---

{% include JB/setup %}

Cocos2d-x调度器为游戏提供定时事件和定时调用服务。**所有Node对象**都知道如何调度和取消调度事件。

游戏中我们经常会随时间的变化而做一些逻辑判断，如碰撞检测。为了解决以上问题，我们使用调度器，使得游戏能够更好的处理动态事件。Cocos2d-x提供了多种调度机制，在开发中我们通常会用到3种调度器。

<!-- more -->

###1.默认调度器(schedulerUpdate)
#####使用背景：
该调度器是使用Node的刷新事件update方法，该方法在每帧绘制之前都会被调用一次。由于每帧之间时间间隔较短，所以每帧刷新一次已足够完成大部分游戏过程中需要的逻辑判断。

Cocos2d-x中Node默认是没有启用update事件的，因此你需要重载update方法来执行自己的逻辑代码。

通过执行schedulerUpdate()调度器每帧执行 update方法，如果需要停止这个调度器，可以使用方法。

#####开启方法：

    scheduleUpdate();

#####关闭方法：

	unschedulerUpdate();


###2.自定义调度器(scheduler)
#####使用背景：
游戏开发中，在某些情况下我们可能不需要频繁的进行逻辑检测，这样可以提高游戏性能。所以Cocos2d-x还提供了自定义调度器，可以实现以一定的时间间隔连续调用某个函数。

由于引擎的调度机制，自定义时间间隔必须大于两帧的间隔，否则两帧内的多次调用会被合并成一次调用。所以自定义时间间隔应在0.1秒以上。

#####开启方法：

```cpp
scheduler(SEL_SCHEDULE selector, float interval, unsigned int repeat, float delay);
```

#####关闭方法：

```cpp
unschedule(SEL_SCHEDULE selector, float delay)。
```

###3.单次调度器(schedulerOnce)
#####使用背景：
游戏中某些场合，你只想进行一次逻辑检测，Cocos2d-x同样提供了单次调度器。

#####开启方法：

```cpp
scheduleOnce(SEL_SCHEDULE selector, float delay);
```

#####关闭方法：

```cpp
unschedule(SEL_SCHEDULE selector, float delay);
```
