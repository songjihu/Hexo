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

## Rain Drips Shader
本部分实现了雨集聚成水流后从物体侧面流下的效果

### 整体思路：
1. 水流的法线与上一节类似，存储在一张贴图的RG通道中，B通道保存了水流的Mask，A通道保存水流形成时间的offset，贴图下载：[rain_drips.tga](http://82.156.182.226:8099/img/my/rain_drips.tga)
2. 水流在向下流动时，某一时刻，顶端到底部有一个渐变的过程，这需要一张额外的贴图，其中存储了一张一维纹理进行Mask，贴图下载：[rain_drip_mask.tga](http://82.156.182.226:8099/img/my/rain_drip_mask.tga)
3. 由于水需要从物体的侧面向下流，因此以上贴图的UV在采样时，需要将坐标切换到侧面；大致思路是使用Sign函数将世界法线截断，然后仅保留R通道或G通道的值（即XY方向的值），此时侧面取到了1。

### 1. 取UV
在侧面取贴图：假设为正方体，其中与YZ平面平行的2个面使用Mask(G B)得到的值作为UV，其余4个平面使用Mask(R B)的值，使用Lerp函数混合，Alpha值的计算过程（法线取Abs Round Mask）实现了上述逻辑。
<div align="center">
<img src="http://82.156.182.226:8099/img/my/QQ截图20210426094230.jpg
" width="90%" height="80%" >
</div>
<br>

### 2. 遮罩
同样的，只需要对雨滴的水流效果计算一个Mask，然后与纹理中存储的Normal相乘即可。

水流的Mask计算：首先，纹理Rain_Drips中的B通道，已经存储了水流的遮罩，但实际使用时需要进行动态的遮罩。因此这里使用了一张一维渐变纹理来模拟水流的上下不均匀，再与原先的遮罩相乘得到水流的遮罩。这张一维纹理的遮罩的UV随时间变化，就可以得到动态的水流。

为了更一步的改进效果，让每一道水流以不同的速度流下，这里使用了Rain_Drips中的Alpha通道存储的值作为offset去将2个小数混合，得到的结果再与Time相乘，就得到了不同的速度。

一维纹理的采样，U值的大小并不影响结果，V值根据上一步得到值与世界坐标的Z值相乘（Z值与一个小数相乘的作用是将2个相加的数字缩放到同一规模）。

最终2个采样值相乘，得到最终的Mask。
<div align="center">
<img src="http://82.156.182.226:8099/img/my/QQ截图20210426155511.jpg
" width="100%" height="100%" >
</div>
<br>

### 3. 法线
法线的取值和上一节方法相同
<div align="center">
<img src="http://82.156.182.226:8099/img/my/QQ截图20210426161512.jpg
" width="80%">
</div>
<br>

### 4. 全局遮罩
这里增加了一点，就是我们只希望最后出现的水流效果出现在侧面，顶部和底部不出现，这时候需要根据法线方向的Z值计算一个全局的遮罩。
<div align="center">
<img src="http://82.156.182.226:8099/img/my/QQ截图20210426162000.jpg
" width="80%">
</div>
<br>

### 5. 效果
<div align="center">
<img src="http://82.156.182.226:8099/img/my/QQ录屏2021042616215920214261623242.gif
" width="50%">
</div>
<br>

## Rain Ripples Shader
本节实现了水滴落下后形成的涟漪效果
### 整体思路：
1. 涟漪的起伏和正弦函数Sin十分类似，做法就是用Sin函数的波峰波谷模拟为涟漪的波峰波谷。在UE中，Sine函数的输入为[0,1]，输出为[-1,1]，可以当作一个波纹，在这里的做法是将输入的范围扩大到了[-20,20]后再乘以Pi，作为Sine函数的输入，取得很多波纹，我们根据实际效果使用Clamp函数，乘以Pi之前就截断到了[0,5]，这样可以取得3个波纹。
2. 本节贴图[rain_ripples.tga](http://82.156.182.226:8099/img/my/rain_ripples.tga)的R通道，存储了每个涟漪的渐变值，GB通道存储了法线，A通道存储了一个偏差Mask，保证所有的涟漪不是完全同步出现和变化。

### 1. 遮罩

Time与Alpha通道值相加取小数部分再减一，作为一个[-1,0]的变化的offset，上边的1-x节点的值，从1到0对应了涟漪开始出现到最后结束，即涟漪散开后效果最弱（1-x取0）。

offset取到后，与R通道的渐变值相加，实现一个循环出现的渐变效果；mul 20 扩大了范围，提前clamp在进行Sine运算取到了其中3个波浪纹（其他的clamp值取到的效果不理想）。
<div align="center">
<img src="http://82.156.182.226:8099/img/my/QQ截图20210426191405.jpg
" width="90%">
</div>
<br>

### 2. 法线
法线获取后与Mask相乘
<div align="center">
<img src="http://82.156.182.226:8099/img/my/QQ截图20210426192419.jpg
" width="80%">
</div>
<br>

### 3. 效果

QQ录屏2021042619294220214261931223.gif