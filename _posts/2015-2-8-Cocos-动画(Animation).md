---
layout: post
title: "Cocos-动画(Animation)"
description: "Cocos-动画(Animation)的使用"
keywords: "cocos, animation, 动画"
category: Cocos
tags: [Cocos]
---

{% include JB/setup %}

摘自官方文档：[http://cn.cocos2d-x.org/article/index?type=cocos2d-x&url=/doc/cocos-docs-master/manual/framework/native/v3/action/zh.md](http://cn.cocos2d-x.org/article/index?type=cocos2d-x&url=/doc/cocos-docs-master/manual/framework/native/v3/action/zh.md)

####Animation

以帧动画形式实现动画效果，以下代码用两种方法实现精灵帧动画效果：

<!-- more -->

```cpp
//手动创建动画
auto animation = Animation::create();
for( int i=1;i<15;i++)
{
    char szName[100] = {0};
    sprintf(szName, "sprite_%02d.png", i);
    animation->addSpriteFrameWithFile(szName);
}
 
animation->setDelayPerUnit(2.8f / 14.0f);
animation->setRestoreOriginalFrame(true);
 
auto action = Animate::create(animation);
sprite->runAction(Sequence::create(action, action->reverse(), NULL));
 
//文件创建动画
auto cache = AnimationCache::getInstance();
cache->addAnimationsWithFile("animation.plist");
auto animation2 = cache->getAnimation("dance_1");
 
auto action2 = Animate::create(animation2);
sprite->runAction(Sequence::create(action2, action2->reverse(), NULL));
```

动画创建后需要一个动画播放器来播放这些动画，这个播放器就是Animate。