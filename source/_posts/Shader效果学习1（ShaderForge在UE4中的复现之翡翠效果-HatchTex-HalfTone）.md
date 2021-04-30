---
title: Shader效果学习1
author: songjihu
avatar: 'http://82.156.182.226:8099/Avatar/avatar.jpg'
authorLink: 'http://82.156.182.226'
categories: 
    - [Unity]
    - [UE4]
comments: true
date: 2020-12-03 17:24:42
authorAbout:
authorDesc:
tags:
keywords:
description: ShaderForge在UE4中的复现之翡翠效果+HatchTex+HalfTone
photos: https://img-blog.csdnimg.cn/20201208103226726.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MDA1NDk4,size_16,color_FFFFFF,t_70
---

## 1.背景
在《庄懂的技术美术入门课(美术向)》Lesson1课后作业中，一位学员制作了类似翡翠效果的卡通渲染Shader，效果图如下所示：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203164449458.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MDA1NDk4,size_16,color_FFFFFF,t_70)现在想要做的事情是在UE4中复现这个渲染效果，效果图如下所示：![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203165553951.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MDA1NDk4,size_16,color_FFFFFF,t_70)
## 2.实现过程
根据Unity中实现的Shader，在UE4的材质连连看中复刻。
Shader Forge
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203170100882.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MDA1NDk4,size_16,color_FFFFFF,t_70)UE4材质
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203171539178.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MDA1NDk4,size_16,color_FFFFFF,t_70)
其中遇到的问题：
1.更改材质中的着色模型部分为无光照，否则总有自带的光照效果。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203171653699.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MDA1NDk4,size_16,color_FFFFFF,t_70)

2.LightDir函数在UE4中没有提供，查询一些资料后（ [UE4 材质里如何获取到平行光方向](https://answer.uwa4d.com/question/5d0276a318013226f621cc9f)），得到解决方案为利用蓝图（关卡蓝图）获得平行光方向，传给材质参数集，再从材质中读取，实现动态更新光照的方向（目前只考虑了平行光）。
获取方式如下：![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203170901276.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MDA1NDk4,size_16,color_FFFFFF,t_70)动态更新（关卡蓝图中实现，但平行光方向好像一般不变，除非在游戏场景中由日落日升的效果）：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201203171220497.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MDA1NDk4,size_16,color_FFFFFF,t_70)
3.UE4中颜色的通道只能为正数，这里微调两者中有差异的参数。
## 3.完结撒花~~
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201208103226726.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MDA1NDk4,size_16,color_FFFFFF,t_70)![在这里插入图片描述](https://img-blog.csdnimg.cn/20201208103255228.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MDA1NDk4,size_16,color_FFFFFF,t_70)









