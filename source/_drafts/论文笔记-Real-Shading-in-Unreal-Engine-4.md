---
title: 论文笔记 Real Shading in Unreal Engine 4
tags:
---

## 材质模型

我们的材质模型是迪士尼的简化，着重于实时渲染的效率。限制参数的数量对于充分利用GBuffer极其重要，还有减少纹理的存储获取、最小化在像素着色器中的混合不同材质层的消耗。

基础的材质模型有以下部分：BaseColor Metallic Roughness Cavity

前3个和迪士尼模型中的概念相同。Cavity用于强调阴影，当一个图形小于阴影系统一次可以处理的
