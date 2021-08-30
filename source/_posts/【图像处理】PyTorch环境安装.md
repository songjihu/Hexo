---
title: 【图像处理】PyTorch环境安装
date: 2021-08-23 18:48:29
categories: 
    - [图像]
author: songjihu
avatar: 'http://82.156.182.226:8099/Avatar/avatar.jpg'
authorLink: 'http://82.156.182.226'
description: 图像处理之PyTorch环境安装
photos: http://82.156.182.226:8099/img/banner/4-26-banner-4-26-header-1920x848-v2-1920x848-674151069.png
mathjax: true

---

## 0.前言
现在新开坑入门深度学习之图像处理的方向，基本从零开始，以此系列记录学习过程。
## 1. 安装ANACONDA
[官网下载地址](https://www.anaconda.com/products/individual)
<div align="center">
<img src="http://82.156.182.226:8099/img/my/QQ截图20210823144652.jpg
" width="60%" height="70%" >
<center></center>
<br/>
</div>

### 1.1 安装PyChorm
[官网下载地址](https://www.jetbrains.com/lp/pycharm-anaconda/?=)
<div align="center">
<img src="http://82.156.182.226:8099/img/my/QQ截图20210823145237.jpg
" width="60%" height="70%" >
<center></center>
<br/>
</div>

### 1.2 在PyChorm中使用虚拟环境（2.3.4完成后）
<div align="center">
<img src="http://82.156.182.226:8099/img/my/QQ截图20210823160419.jpg
" width="100%" height="70%" >
<center></center>
<br/>
</div>

<div align="center">
<img src="http://82.156.182.226:8099/img/my/QQ截图20210823160440.jpg
" width="100%" height="70%" >
<center></center>
<br/>
</div>

<div align="center">
<img src="http://82.156.182.226:8099/img/my/QQ截图20210823160510.jpg
" width="80%" height="70%" >
<center>原本标红的位置消失了，正确的找到了引用的依赖</center>
<br/>
</div>

## 2. 创建环境

<div align="center">
<img src="http://82.156.182.226:8099/img/my/QQ截图20210823151029.jpg
" width="60%" height="70%" >
<center>创建并切换到pytorch环境</center>
<br/>
</div>

```bash
conda create -n pytorch python=3.6
conda info --envs
conda activate pytorch
```
## 3. 获取安装命令
[地址](https://pytorch.org/get-started/locally/)
<div align="center">
<img src="http://82.156.182.226:8099/img/my/QQ截图20210823151456.jpg
" width="70%" height="70%" >
<center>勾选配置，获得安装命令并运行</center>
<br/>
</div>

## 4. 验证安装结果
如下在环境中进入python  
import torch运行后无报错  
运行命令torch.cuda.is_available()得到True结果表示可以使用GPU

```bash
done
(pytorch) PS C:\Users\Dell> python
Python 3.6.13 |Anaconda, Inc.| (default, Mar 16 2021, 11:37:27) [MSC v.1916 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> import torch
>>> torch.cuda.is_available()
True
>>>
```

