---
layout: post
title: "cocos2d-x基本控件-Label"
description: 本篇文章讲述cocos2d-x中三种Label相关知识"
keywords: "cocos2d-x、 label"
category: cocos2d-x
tags: [cocos2d-x, label]
---

{% include JB/setup %}

##故事梗概
在cocos中，有三种Label，分别为
	LabelTTF——TTF(True Type Font)文本标签控件，使用*.ttf字体文件
	LabelAtlas——自定义字体文本标签控件，使用
	LabelBMFont——位图文本标签控件
3.0版本后，将这三种标签控件集合到一个控件中——Label，虽然合到一块儿了，但是本质上还是三种，不同的创建方法就可以创建不同的Label。

<!-- more -->
								
##三种Label
L