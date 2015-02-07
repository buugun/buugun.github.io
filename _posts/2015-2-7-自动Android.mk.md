---
layout: post
title: "自动Android.mk"
description: "Android.mk自动更新需要编译的源文件的方法"
keywords: "Android.mk"
category: 小技巧
tags: [Android.mk]
---

{% include JB/setup %}

Cocos默认生成的Android.mk文件需要自己手动添加需要编译的cpp或c文件，数量少还好说，数量多起来的话，再手动添加显然是浪费精力。

所以这里记一下Android.mk里自动添加cpp或c文件的方法。

<!-- more -->

参考自：[http://blog.csdn.net/qqmcy/article/details/39551979](http://blog.csdn.net/qqmcy/article/details/39551979)

感谢这位朋友

====

###使用方法：
将原Android.mk中的
`
LOCAL_SRC_FILES := hellocpp/main.cpp \
                   ../../Classes/AppDelegate.cpp \
                   ../../Classes/HelloWorldScene.cpp
`

替换为
```
#auto
#traverse all the directory and subdirectory  
define walk  
  $(wildcard $(1)) $(foreach e, $(wildcard $(1)/*), $(call walk, $(e)))  
endef  
   
#traverse Classes Directory  
ALLFILES = $(call walk, $(LOCAL_PATH)/../../Classes)  
   
FILE_LIST := hellocpp/main.cpp  
FILE_LIST += $(filter %.cpp, $(ALLFILES))  
FILE_LIST += $(filter %.c, $(ALLFILES))  
   
#source file will be compiled  
LOCAL_SRC_FILES := $(FILE_LIST:$(LOCAL_PATH)/%=%)
#end auto
```