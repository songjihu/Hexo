---
title: Games202学习笔记1：Recap of CG Basics
date: 2022-3-16 15:32:25
categories: 
    - [图形学]
    - [百人计划]
author: songjihu
avatar: 'http://82.156.182.226:8099/Avatar/avatar.jpg'
authorLink: 'http://82.156.182.226'
description: 一些基本知识复习
photos: http://82.156.182.226:8099/img/my/QQ截图20220316172934.png
mathjax: true

---

学习教程来自:[GAMES202-高质量实时渲染](https://www.bilibili.com/video/BV1YK4y1T7yY?p=2)

# 笔记

## 1. Basic GPU hardware pipeline

<div align="center">
<img src="http://82.156.182.226:8099/img/my/QQ截图20220316154131.png
" width="100%">
</div>
<center>渲染管线</center>
</br>

## 2. OPenGL
是一系列在CPU端调用的API用来调用GPU工作。  
1. 优点：跨平台。
2. 缺点：碎片化，有很多版本。语言风格为C style，非面向对象。

渲染过程类比：
1. 放置物体/模型：模型声明(VBO)，模型变换矩阵(glTranslate等)
2. 放置画架：声明相机(gluPerspective等)，使用/声明一个FrameBuffer
3. 画布和画架关系：一个Pass可以输出多个Texture(MRT)，Fragment Shader决定了其内容
4. 在画布上作画：着色。本课程重点在顶点着色器、片元着色器2个部分

## 3. OpenGL Shading Language (GLSL)
描述着色器如何着色的语言：着色器语言经过编译变为在GPU上执行的汇编语言  
1. HLSL for DX(vertex + pixel)
2. GLSL for OpenGL(vertex + fragment)

一些变量的声明
1. attribute：顶点着色器中的本地变量
2. uniform：固定的全局变量
3. varying：顶点着色器的变量插值后送给片元着色器
4. highp：高精度

### 3.1 Debug
工具：Nsight Graphics、RenderDoc  
方法：打印颜色

## 4. The Rendering Equation
正确的用来描述光线传播的公式

<!-- $$
L_o(p, \omega_o) = L_e(p, \omega_o) + \int_{H^2}^{}f_r(p, \omega_i \to \omega_o) L_i(p, \omega_i)  \cos \theta_i dw_i
$$ --> 

PBR:
<div align="center"><img style="background: white;" src="http://82.156.182.226:8099/img/my/fYkqvQ8c9A.svg"></div>

In Real Time Rendering：对入射光源是否被遮挡(V)单独描述

<!-- $$
L_o(p, \omega_o) = \int_{\Omega^+}^{}L_i(p, \omega_i) f_r(p, \omega_i, \omega_o)  \cos \theta_i V(p, \omega_i) dw_i
$$ --> 

<div align="center"><img style="background: white;" src="http://82.156.182.226:8099/img/my/lwczOFeCIB.svg"></div>

# 作业
作业0：在框架里实现Bulin-Phong，所有代码已给出  
碰到的问题：  
没有渲染出来是因为uViewMatrix和uViewMatrix这2个矩阵没有传过来，需要
1. 在Material.js中this.#flatten_uniforms部分声明这2个参数
2. 在MeshRender.js的Draw函数中定义对应的2个矩阵，然后通过gl.uniformMatrix4fv函数传给下一层（参考作业1中的对应代码）

如果没有对vFragPos进行世界空间的转换而仅仅将aVertexPosition传入，那么原本(52,52,52)的缩放就没有起作用，导致光线的衰减是基于(1,1,1)缩放后的世界空间位置计算的

[最终效果](http://songjihu.top:8022/HW0/homework0/homework0/)

