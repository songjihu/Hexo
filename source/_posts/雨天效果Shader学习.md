---
title: 雨天效果Shader学习
date: 2021-04-19 16:12:34
categories: UE4
author: songjihu
avatar: 'http://songjihu.top:8099/Avatar/avatar.jpg'
authorLink: 'http://songjihu.top'
description: UE4材质蓝图制作雨天效果
photos: http://songjihu.top:8099/img/my/QQ截图20210419162949.jpg
---

学习教程来自:[Rain Wetness Shader - UE4 Materials 101](https://www.youtube.com/watch?v=5eyq2FJ6lig)

UE4版本：4.26.2
## Episode 13: Rain Wetness Shader
基于UE4 Start Content中的地砖材质 M_Brick_Clay_Old ，对原输出的Base Color,Roughness增加计算，额外添加了Specular的计算（原本为默认值0.5）。

大致思路如下：

1. 减少材质的Roughness，Specular（教程中按照0.07，0.3的比例减少），减少后与之前的值进行Lerp在分别输出，Lerp的Alpha通道控制积水程度。

2. 增加BaseColor的饱和度（Desaturation节点Fraction中使用负数来增加饱和度），减少亮度（Mul 0.5），与原本BaseColor混合。
3. 以上步骤中都进行了Lerp操作，其中Alpha通道的设置和计算需要多理解。

### Step 1
创建材质函数Wetness，如下进行连接
![](http://songjihu.top:8099/img/my/QQ截图20210419164921.jpg)
### Step 2
在复制得到的材质中，添加材质函数
![](http://songjihu.top:8099/img/my/QQ截图20210419165628.jpg)

其中BaseColor和Roughness为原本材质中的输出，现在作为材质函数的输入。

Specluar设为0.5参加计算。

WetMask单独作为Roughness和Specular进行Lerp时的Alpha控制混合的程度；Porousness与WetMask相乘后并Saturate后，控制积水造成的增加饱和度和变暗效果的程度。
### Step 3
材质球
![](http://songjihu.top:8099/img/my/QQ截图20210419173357.jpg)
实际效果
![](http://songjihu.top:8099/img/my/QQ截图20210419162949.jpg)

## Episode 14: Rain Drops Shader
Rain Drops雨滴效果实现：


