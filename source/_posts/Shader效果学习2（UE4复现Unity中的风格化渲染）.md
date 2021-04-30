---
title: Shader效果学习2
author: songjihu
avatar: 'http://82.156.182.226:8099/Avatar/avatar.jpg'
authorLink: 'http://82.156.182.226'
categories: 
    - [Unity]
    - [UE4]
comments: true
date: 2021-02-03 16:03:13
authorAbout:
authorDesc:
tags:
keywords:
description: UE4复现Unity中的风格化渲染
photos: https://img-blog.csdnimg.cn/20210203154711795.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MDA1NDk4,size_16,color_FFFFFF,t_70
---

# Shader效果学习
<font color=#999AAA >之前的随笔：
[Shader效果学习1（ShaderForge在UE4中的复现之翡翠效果+HatchTex+HalfTone）](https://www.csdn.net/).
<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">

@[TOC](随笔目录)

</font>

<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">

# 前言

<font color=#999AAA >随笔记录了在《庄懂的技术美术入门课(美术向)》学习过程中，一些Unity渲染效果在UE4引擎中的复现。</font>

<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">

Unity中的效果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210203154524919.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MDA1NDk4,size_16,color_FFFFFF,t_70)
<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">

UE4中的效果：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210203154711795.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MDA1NDk4,size_16,color_FFFFFF,t_70)

<hr style=" border:solid; width:100px; height:1px;" color=#000000 size=1">


# 一、UE4中的LightDir，ViewDir，NormalDir


参考资料：[Shader学习 （12）使用Unity和UE4实现三个经典光照模型](https://zhuanlan.zhihu.com/p/172496451)
大佬的文章指出了如何在UE4中实现Lambert、Phong、Blinn-Phong光照模型，其中基础的光照变量对照如下：

| UE4 | Unity |
|--|--|
| AtmosphericLightVector | LightDir |
| CameraDirectionVector | ViewDir |
| PixelNormalWS | NormalDir |


# 二、Shader Forge中的Multiply
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210203142852297.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MDA1NDk4,size_16,color_FFFFFF,t_70)实际使用时，这里的Multiply计算得到的颜色对比UE4中同样的Multiply节点偏暗，需要在UE4中再次乘以0.5效果较为接近。左：Unity，右：UE4
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210203143845437.png =240x484)![在这里插入图片描述](https://img-blog.csdnimg.cn/20210203143814758.png)



# 三、在UE4中使用更多的参数
1. 由于引擎的不同，相同的RGB值会得到不同的颜色表现，需要直接将颜色面板中的颜色使用小吸管工具得到。例如下图中RGB=0.216的颜色与Unity中RGB=0.5的颜色相同。
2. 完全相同的过程可能导致不同的结果，在Multiply和Lerp的过程中，最终结果相差越来越远。引入更多的参数，手动调节每一部分的结果更加便捷。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210203154849650.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MDA1NDk4,size_16,color_FFFFFF,t_70)
# 四、蓝图
## 1. 漫反射部分
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210203155858129.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MDA1NDk4,size_16,color_FFFFFF,t_70)
## 2. 高光部分
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210203160111416.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MDA1NDk4,size_16,color_FFFFFF,t_70)
## 3. 合并结果
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210203160200734.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MDA1NDk4,size_16,color_FFFFFF,t_70)



