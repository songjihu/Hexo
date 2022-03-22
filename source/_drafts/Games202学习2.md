---
title: Games202学习笔记：Real-Time Shadows
date: 2022-3-17 16:10:53
categories: 
    - [图形学]
    - [百人计划]
author: songjihu
avatar: 'http://82.156.182.226:8099/Avatar/avatar.jpg'
authorLink: 'http://82.156.182.226'
description: 实时阴影实现
photos: http://82.156.182.226:8099/img/my/QQ截图20220316172934.png
mathjax: true

---

学习教程来自:[GAMES202-高质量实时渲染](https://www.bilibili.com/video/BV1YK4y1T7yY?p=3)

# 笔记

## 1. Shadow Mapping
2个Pass：
1. The Light Pass：从光源出发生成Shadow Map
2. The Camera Pass：拿到上一Pass的SM并对比深度，渲染


优点：不需要场景的几何信息，只需要Shadow Map

缺点：见下

### 1.1 自阴影 Self Collusion

精度问题导致对比深度时产生误差  
QQ截图20220317163456.png 来自课程PPT

解决办法：
1. 增加一个Bias来弥补精度的误差，但可能导致阴影与物体脱离  
QQ截图20220317164840.png
2. 取正面最小深度和背面的次小深度折中作为深度值，但最小和次小的排序问题会导致复杂度上升  
QQ截图20220317164516.png

### 1.2 走样 Aliasing

shadow map分辨率导致的走样问题  
解决办法：见表格

### 1.3 小结

一些缺点和解决办法（部分参考自[技术美术知识学习4300：实时阴影](http://songjihu.top/2021/09/23/%E6%8A%80%E7%BE%8E%E7%9F%A5%E8%AF%86%E5%AD%A6%E4%B9%A04300/)）：
|方法名称|方法|解决的问题|
|:---:|:---:|:---:|
|深度偏移|使用一个偏移值来避免深度比较时产生的误差|自阴影|
|法线偏移|沿法线方向进行偏移|自阴影|
|Second-depth shadow mapping|取2次计算的深度折中作为深度值|自阴影|
|透视投影|在Shadow Map生成时进行透视投影，以保持均匀性的一致|透视走样
|级联阴影映射|从近到远划分视锥体，得到相同大小的Shadow Map|透视走样
|PCF滤波|对阴影贴图滤波，得到shadow值|重采样|
|动态分辨率|--|走样|

## 2. 数学部分 The math behind shadow mapping

### 2.1 近似 Approximation in RTR

<!-- $$
\int_{\Omega}^{}f(x)g(x)\mathrm{d}x \approx \frac{\int_{\Omega}^{}f(x)\mathrm{d}x}{\int_{\Omega}^{}\mathrm{d}x} \cdot \int_{\Omega}^{}g(x)\mathrm{d}x
$$ --> 

<div align="center"><img style="background: white;" src="http://82.156.182.226:8099/img/my/qtpPhNSpSQ.svg"></div>

趋近的2个条件：
1. 积分的范围很小的时候
2. g(x)足够光滑的时候

<!-- $$
L_o(p, \omega_o) = \int_{\Omega^+}^{}L_i(p, \omega_i) f_r(p, \omega_i, \omega_o)  \cos \theta_i V(p, \omega_i) \mathrm{d}w_i
$$ --> 


<div align="center"><img style="background: white;" src="http://82.156.182.226:8099/img/my/TTepgAfYjI.svg"></div>

<!-- $$
L_o(p, \omega_o) \approx \frac{\int_{\Omega^+}^{}V(p, \omega_i) \mathrm{d}w_i}{\int_{\Omega^+}^{} \mathrm{d}w_i} \cdot \int_{\Omega^+}^{}L_i(p, \omega_i) f_r(p, \omega_i, \omega_o)  \cos \theta_i \mathrm{d}w_i
$$ --> 

对应上述的2个趋近条件：
1. point / directional lighting：点光源或方向光源 
2. diffuse bsdf / constant radiance area lighting：面光源或Shading Point为diffuse
<div align="center"><img style="background: white;" src="http://82.156.182.226:8099/img/my/qKuJMG20Wf.svg"></div>

## 3. Percentage closer soft shadows (PCSS)
软阴影：从无到有存在过度

### 3.1 Percentage Closer Filtering (PCF)
在[技术美术知识学习4300：实时阴影 2.2.2 重采样](http://songjihu.top/2021/09/23/%E6%8A%80%E7%BE%8E%E7%9F%A5%E8%AF%86%E5%AD%A6%E4%B9%A04300/)中有所介绍  
阴影边缘的对比计算时进行滤波，在比较时，同时比较SM周围的fragment并将结果取均值  
PCF滤波，对滤波核的每一个采样点，对比中间的值后划分为blocked和visible这2种状态，输出shadow=visible/(visible+blocked)。实现方式有很多种（不同的采样个数、不同的滤波函数）  
<div align="center">
<img src="http://82.156.182.226:8099/img/my/
QQ截图20210923204029.png
" width="100%" height="70%" >
<center></center>
<br/>
</div>

### 3.2 从PCF到软阴影 PCSS
根据物体到阴影接受物的距离（Blocker Distance）定义不同大小的filter size，进而产生软阴影

QQ截图20220317201416.png 

相似三角形原理：Blocker的深度决定了
<!-- $$
\mathrm{w}_{Penumbra} = (d_{Receiver}-d_{Blocker}) \cdot \mathrm{w}_{Light} / d_{Blocker}
$$ --> 

<div align="center"><img style="background: white;" src="http://82.156.182.226:8099/img/my/fEvZPiY8GA.svg"></div>

### 3.3 PCSS算法
过程如下：
1. Blocker Search：在Shading Point的周围一定区域内计算Blocker的平均深度
2. Penumbra estimation：根据上一步的结果决定Filter的大小
3. PCF滤波

Blocker Search的区域大小取值依据：
1. Light Size越大，区域越大
2. Light距离接收者越近，区域越大

QQ截图20220317204230.png

## 4. More on PCF and PCSS

$$
V(x) = \sum_{q\in}^{} \chi
$$

# 作业



