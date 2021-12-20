---
title: 【Niagara】解析Grid2D_Gas_Explosion_System
date: 2021-12-01 08:47:07
categories: 
    - [Niagara]
    - [特效]
author: songjihu
avatar: 'http://82.156.182.226:8099/Avatar/avatar.jpg'
authorLink: 'http://82.156.182.226'
description: 抄一遍代码顺便记笔记
photos: http://82.156.182.226:8099/img/banner/4-26-banner-4-26-header-1920x848-v2-1920x848-674151069.png
mathjax: true

---

# 0. 前言

<div align="center">
<img  src="http://82.156.182.226:8099/img/my/QQ截图20211201090707.jpg
" width="100%" height="70%" >
<center></center>
<br/>
</div>

代码来自Niagara插件（UE5.0），效果如图

<div align="center">
<img  src="http://82.156.182.226:8099/img/my/QQ录屏202112010913082021121913461.gif
" width="100%" height="70%" >
<center></center>
<br/>
</div>

本篇试图在UE4.26中抄一遍代码，分析实现过程并做笔记

# 1. 系统配置部分
<div align="center">
<img  src="http://82.156.182.226:8099/img/my/QQ截图20211208084133.jpg
" width="100%" height="70%" >
<center></center>
<br/>
</div>

1. 系统设置：增加类型为Object的用户参数SimRT，可传参用于Debug
2. 系统生成：无
3. 系统更新：Loop Duration从默认的5调整至3

# 2. 粒子发射器 Source 部分

<div align="center">
<img  src="http://82.156.182.226:8099/img/my/QQ录屏202112010926372021121927332.gif
" width="80%" height="70%" >
<center></center>
<br/>
</div>

source发射器生成了爆炸的粒子，是整个爆炸效果的基础  
分为3个部分：发射器部分、粒子部分、渲染部分

## 2.1 发射器部分
分为发射器设置、发射器生成、发射器更新3部分

### 2.1.1 发射器设置
<div align="center">
<img  src="http://82.156.182.226:8099/img/my/QQ截图20211208084706.jpg
" width="100%" height="70%" >
<center></center>
</div>
对于默认发射器设置有以下变化：  
1. GPU生成：更优的性能、支持更多的粒子
2. 启用模拟阶段：需要勾选1才能启用，可在此阶段对Grid2D进行计算
3. 公开到库：经测试取消勾选并不影响第二个发射器对其粒子的读取，暂意义不明

### 2.1.2 发射器生成
无

### 2.1.3 发射器更新
<div align="center">
<img  src="http://82.156.182.226:8099/img/my/QQ截图20211201095849.jpg
" width="100%" height="70%" >
<center>发射器每2s发射一次，一次发射所有粒子（数量为范围随机整数）</center>
<br/>
</div>

## 2.2 粒子部分

### 2.2.1 粒子生成
<div align="center">
<img  src="http://82.156.182.226:8099/img/my/QQ截图20211208085540.jpg
" width="100%" height="70%" >
<center>配置初始Lifetime、Color、Mass、Size，给初速度和生成位置</center>
</div>

### 2.2.2 粒子更新
<div align="center">
<img  src="http://82.156.182.226:8099/img/my/QQ截图20211208094534.jpg
" width="100%" height="70%" >
<center></center>
</div>

1. 粒子状态：当Lifetime结束Kill
2. Curl Noise、Gravity、Drag：扰动、重力、向中心的拉力
3. Scale Color：颜色逐渐变弱

<div align="center">
<img  src="http://82.156.182.226:8099/img/my/QQ截图20211208095008.jpg
" width="100%" height="70%" >
<center></center>
</div>

4. 碰撞检测：使用GPU Depth Buffer
5. 解算速度和力：更新粒子速度和位置
6. Fluid Gas Source：输入一些粒子的本身属性，温度和半径均是先变大后变小

第6项来自Fluids_Gas_Source(Niagara Module Script)
<div align="center">
<img  src="http://82.156.182.226:8099/img/my/QQ截图20211208093016.jpg
" width="100%" height="70%" >
<center>将参数输入到粒子的对应属性中（其中velocity输入绑定为粒子自带的速度）</center>
</div>

## 2.3 渲染部分
新建Sprite Render，材质使用自带的DefaultSpriteMaterial

# 3. 粒子发射器 Grid2D_Gas_SmokeFire 部分
