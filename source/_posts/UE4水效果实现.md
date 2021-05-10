---
title: UE4水效果学习
date: 2021-05-10 08:17:10
categories: UE4
author: songjihu
avatar: 'http://82.156.182.226:8099/Avatar/avatar.jpg'
authorLink: 'http://82.156.182.226'
description: UE4 4.26版本新增水效果的使用
photos: http://82.156.182.226:8099/img/my/QQ截图20210507210652.jpg
---

本篇着重点偏向于地形的制作（LandMass+地形材质）

## 地形材质

参考教程：
[Quixel Tutorial: Landscape Blend Material with Megascans in Unreal Engine 4](https://www.youtube.com/watch?v=ozLevdUjr00)

[New Megascans Landscape Blend Material in Unreal Engine 4](https://www.youtube.com/watch?v=esuOUHfRjsE)

主要思路：创建材质蓝图，使用LandLayerBlend节点，混合多个Layer的材质，最后在地形绘制完成后的蓝图笔刷中配置材质以生效

### 1. 导入材质贴图

此案例中共导入3种材质的贴图，分别用作地形基础、河流边缘、河流底部（最后一个字段为ID，可以直接在Magescans中搜索到唯一对应的材质贴图）
LayerA Base: Layered_Rock_2x2_M_ui1jei3dy
<div align="center">
<img src="http://82.156.182.226:8099/img/my/QQ截图20210507182805.jpg" width="100%">
</div>
<br>
LayerB Surround the river: Damp_Rocky_Path_2x2_M_uc0kedqkw
<div align="center">
<img src="http://82.156.182.226:8099/img/my/QQ截图20210507182730.jpg" width="100%">
</div>
<br>
LayerC Under the River: Mossy_Ground_2x2_M_ukimchjew
<div align="center">
<img src="http://82.156.182.226:8099/img/my/QQ截图20210507183126.jpg" width="100%">
</div>
<br>

### 2. 创建材质函数

每一层创建一个材质函数并实例化，以用于最终混合
<div align="center">
<img src="http://82.156.182.226:8099/img/my/QQ截图20210507170650.jpg" width="100%">
</div>
<br>
此处以LayerA为例：更改其中每一张Texture的参数名和分组；复制Tilling函数，创建一个TillingA，同样改变其中Tilling变量的参数名。
<div align="center">
<img src="http://82.156.182.226:8099/img/my/QQ截图20210507174917.jpg" width="50%">
</div>
<br>
<div align="center">
<img src="http://82.156.182.226:8099/img/my/QQ截图20210507175006.jpg" width="50%">
</div>
<br>
这样做的好处是，最终的材质蓝图实例化后，可以更便捷的调整其中的各个参数（LayerA B C使用了不同的贴图和偏移） 
<div align="center">
<img src="http://82.156.182.226:8099/img/my/QQ截图20210507181611.jpg" width="70%">
</div>
<br>

### 3. 得到地形材质
材质函数LayerA B C进行混合
<div align="center">
<img src="http://82.156.182.226:8099/img/my/QQ截图20210507182428.jpg" width="80%">
</div>
<br>
实例化后放入对应贴图（目前只使用了其中3张 Albedo Roughness Normal）
<div align="center">
<img src="http://82.156.182.226:8099/img/my/QQ截图20210507182642.jpg" width="70%">
</div>
<br>



## 绘制地形

使用蓝图笔刷，创建一个山地地形
UE4插件 LandMass
参考教程：[Create QUICK Landscapes with Landmass Blueprint Brushes in UE 4.26](https://www.youtube.com/watch?v=GK3KAevUy8E&list=PLcUTPYkj5fo5DjcrbfmY9KNTU_Z3osk26)

### 地形配置

<div align="center">
<img src="http://82.156.182.226:8099/img/my/QQ截图20210506091026.jpg" width="70%">
</div>
<br>

### 使用蓝图笔刷创建雕刻地形

#### CustomBrush_Landmass
创建局部的地形

<div align="center">
<img src="http://82.156.182.226:8099/img/my/QQ图片20210506093421.png" width="100%">
</div>
<br>

1. 右键可添加样条点
2. 按住Alt拖动样条点可增加一个样条点到结束移动的位置
3. 按住Alt拖动整个Brush创建的地形，同理可复制一份，与原地形不相同且重叠时可互相影响
4. 左上角Layer中，新建层，随后在不同的层中创建地形，用于有选择性的混合调整地形

<div align="center">
<img src="http://82.156.182.226:8099/img/my/QQ截图20210506095043.jpg" width="100%">
</div>
<br>

一些配置选项的作用

|选项名|作用|
|:----:|:----:|
|Curl Noise|选项模拟山脉的地形|
|Blurring|较大的数值使得山脉平滑|
|Displacement|使用一张Noise Texture适当的改变地形高度|
|Brush Setting|蓝图Brush的配置|
|Falloff|勾选Cap Shape选项以创建平顶山|

#### CustomBrush_MaterialOnly
创建全局的地形，此Brush同时影响了上一步创建的地形

一些Brush Settings选项的作用

|选项名|作用|
|:----:|:----:|
|Tilling|Nosie采样的伸缩，值越小越平滑|
|Evaluation Scale|迭代的规模，值越小越平滑|
|Evaluation Pre-Offset|迭代时的全局offset，值为1时此Brush效果消失|
|Seed|产生不同的Noise|
|Inverse|Noise值取反（存疑）|
|Octaves|更小的数值，更加平滑的细节|
|Persistence|同上|


适当调整后得到的效果
<div align="center">
<img src="http://82.156.182.226:8099/img/my/QQ截图20210506104945.jpg" width="100%">
</div>
<br>


## 创建水效果
新的水系统可以在添加水效果的同时改变地形，所以不需要像之前那样去雕刻地形了，直接添加water就好了
参考资料：
[Unreal Engine 4.26 Water System Tutorial: The Basics](https://www.youtube.com/watch?v=PEVmWFdkUqk)
[Unreal Engine 4.26 Water System Tutorial: Rivers and Lakes](https://www.youtube.com/watch?v=HvoPUFQos2o)
### 地形材质使用
1. 赋予地形材质
<div align="center">
<img src="http://82.156.182.226:8099/img/my/QQ截图20210507183307.jpg" width="50%">
</div>
<br>

2. 在绘制中设置混合方式
<div align="center">
<img src="http://82.156.182.226:8099/img/my/QQ截图20210507183521.jpg
" width="70%">
</div>
<br>

<div align="center">
<img src="http://82.156.182.226:8099/img/my/QQ截图20210507183739.jpg
" width="50%">
</div>
<br>
接着使用笔刷，调整好强度和衰减绘制即可

3. 绘制完成
<div align="center">
<img src="http://82.156.182.226:8099/img/my/QQ截图20210507210652.jpg
" width="100%">
</div>
<br>

### 创建河流和海洋
试图制作一座海洋中的小岛，中间有一条河流穿过（细节差太多了，慢慢补吧）
<div align="center">
<img src="http://82.156.182.226:8099/img/my/QQ截图20210507211434.jpg
" width="100%">
</div>
<br>
需要注意的是，调整2个系统的衰减边缘，不平整的边缘也可以切换到地形模式，使用雕刻进行微调