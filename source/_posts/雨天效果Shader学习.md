---
title: 雨天效果Shader学习
date: 2021-04-19 16:12:34
categories: UE4
author: songjihu
avatar: 'http://82.156.182.226:8099/Avatar/avatar.jpg'
authorLink: 'http://82.156.182.226'
description: UE4材质蓝图制作雨天效果
photos: http://82.156.182.226:8099/img/my/HighresScreenshot00002.png
mathjax: true
---

学习教程来自:[Rain Wetness Shader - UE4 Materials 101](https://www.youtube.com/watch?v=5eyq2FJ6lig)

UE4版本：4.26.2
## Rain Wetness Shader
基于UE4 Start Content中的地砖材质 M_Brick_Clay_Old ，对原输出的Base Color,Roughness增加计算，额外添加了Specular的计算（原本为默认值0.5）。

### 整体思路：

1. 减少材质的Roughness，Specular（教程中按照0.07，0.3的比例减少），减少后与之前的值进行Lerp在分别输出，Lerp的Alpha通道控制积水程度。

2. 增加BaseColor的饱和度（Desaturation节点Fraction中使用负数来增加饱和度），减少亮度（Mul 0.5），与原本BaseColor混合。
3. 以上步骤中都进行了Lerp操作，其中Alpha通道的设置和计算需要多理解。

### Step 1
创建材质函数Wetness，如下进行连接
![](http://82.156.182.226:8099/img/my/QQ截图20210419164921.jpg)
### Step 2
在复制得到的材质中，添加材质函数
![](http://82.156.182.226:8099/img/my/QQ截图20210419165628.jpg)

其中BaseColor和Roughness为原本材质中的输出，现在作为材质函数的输入。

Specluar设为0.5参加计算。

WetMask单独作为Roughness和Specular进行Lerp时的Alpha控制混合的程度；Porousness与WetMask相乘后并Saturate后，控制积水造成的增加饱和度和变暗效果的程度。
### 效果图

<div align="center">
<img src="http://82.156.182.226:8099/img/my/QQ截图20210419173357.jpg" width="70%" height="70%" >
<center>材质球</center>
<br/>
</div>
<div align="center">
<img src="http://82.156.182.226:8099/img/my/QQ截图20210419162949.jpg" width="70%" height="70%" >
</div>

<center>实际效果</center>
<br/>

## Rain Drops Shader
Rain Drops雨滴效果实现

### 整体思路：
1. 所需贴图：[rain_drops.tga](http://82.156.182.226:8099/img/my/rain_drops.tga)
2. RG通道->法线贴图；B通道->雨滴出现Mask；A通道->雨滴动态静态Mask
3. 当雨点落在物体上时，落点处需要改变2个值，法线Normal和粗糙度Roughness；贴图提供了法线的方向，粗糙度用0.1去Power之前计算的Mask值(0消失-1出现)后再用1-x得到，这样得到的值在0左右，且可以从1快速的变化到0
<div align="center">
<img src="http://82.156.182.226:8099/img/my/QQ截图20210423182801.jpg" width="50%" height="50%" >
</div>
<div align="center"><img style="background: white;" src="https://render.githubusercontent.com/render/math?math=1-%5Cleft(x%5Cright)%5E%7B0.1%7D"></div>
 
<!-- $$
1-\left(x\right)^{0.1}
$$ --> 


### 1. 法线
首先读取贴图，其中Panner预留了移动UV的功能
<div align="center">
<img src="http://82.156.182.226:8099/img/my/QQ截图20210423200425.jpg" width="90%" height="90%" >
</div>

从RG通道计算normal方向，并与最终得到的Mask相乘

<div align="center">
<img src="http://82.156.182.226:8099/img/my/QQ截图20210423200816.jpg" width="90%" height="90%" >
</div>

### 2. 遮罩
动态雨滴遮罩：从B通道取出值后进行随机（因为这张贴图的遮罩值的随机性效果不是很好，会有大量的雨点几乎同时出现，所以2次进行了随机计算），与时间做差后保留小数部分，再与A通道的动态雨滴遮罩（1表示动态，反之为静态）做乘积

静态雨滴遮罩：A通道中为0的部分为静态小雨滴，经过计算后变为1，由于精度的关心，power函数将非1的部分全部归为了0，从而得到静态的部分


<div align="center">
<img src="http://82.156.182.226:8099/img/my/QQ截图20210423202413.jpg" width="100%" height="100%" >
</div>
<br>


上面2者相加得到了雨滴部分的遮罩，但是我们希望只有物体的正面有雨滴，所以需要剔除掉世界法线方向向上（Z值小于0）的部分，其中saturate截掉了小于0的部分，后边的mul可以限制是不是所有存在向上分量的部分都要有雨滴（目前是的）

<div align="center">
<img src="http://82.156.182.226:8099/img/my/QQ截图20210423203229.jpg" width="70%" height="70%" >
</div>
<br>

### 3. 合并结果
将最后遮罩得到的输出与贴图中存储的法线值相乘后得到Normal；遮罩还参加计算得出Roughness；连接如图：
<div align="center">
<img src="http://82.156.182.226:8099/img/my/QQ截图20210423203941.jpg" width="90%" height="90%" >
</div>
<br>

### 4. 效果图
<div align="center">
<img src="http://82.156.182.226:8099/img/my/QQ录屏2021042320471020214232047541.gif
" width="50%" height="50%" >
</div>
<br>