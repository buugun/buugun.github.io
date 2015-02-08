---
layout: post
title: "cocos-Auto-batching"
description: "介绍了3.0版本中处理大批量精灵的绘制的新机制——Auto-batching"
keywords: "cocos, auto batching"
category: cocos
tags: [cocos2d-x, auto batching]
---

{% include JB/setup %}

官方文档：[https://github.com/chukong/cocos-docs/blob/master/manual/framework/native/v3/auto-batching/zh.md](https://github.com/chukong/cocos-docs/blob/master/manual/framework/native/v3/auto-batching/zh.md)

相信看了官方文档后都会对这个东西有个了解，所以这里只记录我觉得可以注意一下的地方。

<!-- more -->

####Auto-batching 的渲染逻辑

官方文档的叙述：

	1. 遍历渲染命令队列，这时候会有一个变量，用来保存渲染命令里的材质ID

	2. 遍历过程中就拿当前渲染命令的材质ID和上一个的材质ID对比

	3. 如果发现是一样的，那就不进行渲染，保存一下所需的信息，继续下一个遍历。
	   如果这时候发现当前材质ID和上一个材质ID不一样，那就开始渲染，这就算是一个渲染批次了。

	如图：
![Auto-batching](/assets/images/auto-batching/auto-batching.png)

可以简单理解为将**渲染命令队列**中**相邻的**且**材质ID相同的**多个draw打包为一个draw。

这里提一下：

	**渲染命令队列并不一定是代码中添加精灵的顺序**，因为在渲染之前，Auto-batching会自动地**根据zOrder**对渲染命令队列进行重新排序，优化渲染效率。

	所以如果没有办法在代码上保持添加节点的有序，就可以为节点设置zOrder(eg. sprite->setZOrder(1);)。

####使用精灵帧表单——官方文档中推荐使用的方式（摘写）
使用精灵帧表单，加载生成添加不同的精灵。但是各个精灵的材质都是一样的，满足Auto-batching的条件。此时的渲染批次为2.(首先，即使我一个精灵也不创建，渲染批次也至少是1,加上刚刚这重复添加的精灵的渲染)

```cpp
SpriteFrameCache::getInstance()->addSpriteFramesWithFile("MatrixLayer.plist");

Size winSize = Director::getInstance()->getWinSize();
for(int i = 0; i < 10000; i++)
{
    char buf[64];
    sprintf(buf,"Item%dn.png", i%5 + 1);
    SpriteFrame **frame= SpriteFrameCache::getInstance()->getSpriteFrameByName(buf);
    Sprite **sprite = Sprite::createWithSpriteFrame(frame);
    sprite->setPosition(Point(CCRANDOM_0_1() ** winSize.width, 0 + CCRANDOM_0_1() ** winSize.height));
    this->addChild(sprite);
}
```