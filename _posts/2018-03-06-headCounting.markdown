---
layout:     post
title:      "End-to-end people detection in crowded scenes论文笔记"
date:       2018-03-06 09:21:02
author:     "Pumay"
header-img: "img/star.png"
catalog: true
tags:
    - head counting
    - cnn
    
---


# 一.简介


based on : decoding an image into a set of people detections.

input : an image 

output : a set of distinct detection hypotheses

![](/img/An overview of model .png)

整体流程如上图：
1. 首先通过卷积架构（ Christian Szegedy, Wei Liu, Yangqing Jia, Pierre Sermanet, Scott Reed, Dragomir Anguelov,Dumitru Erhan, Vincent Vanhoucke, and Andrew Rabinovich. Going deeper with convolutions.CoRR, abs/1409.4842, 2014.）将图像编码为高维描述符；
2. 然后将该特征解码为一组边界框，作为预测可变长度输出的核心机制，建立一个LSTM单元的循环网络；
3. 在整个图像的跨越区域将其转换成具有1024个维度特征的网格描述符，这1024维向量汇总了区域的内容并携带了关于对象位置的丰富信息；
4. LSTM从该信息源获取并且在区域的解码中充当控制器，每一步，LSTM输出新的边界框和对应的置信度，即在该位置处将发现先前未检测到的人。这些边界框将按照置信度降序生成；
5. 当LSTM在具有高于预定阈值的置信度的区域中不能再找到另一个框时，就会产生停止符号；
6. 输出序列将被拼接为该区域中所有对象实例的最终描述。

# 二.loss function

在每次重复时，LSTM输出一个对象边界框b = {b_pos，b_c}，其中b_pos =（b_x，b_y，b_w，b_h）∈R^4 是边界框的相对位置、宽度和高度，b_c∈[ 0,1]是置信度的真值。低于预定阈值（例如0.5）的置信度值在测试时将被视为停止符号。较高的边界框置信度b_c应该指示该边界框更可能对应于真阳性。我们将相应的标准真值边界框集合表示为G = {b^i | i = 1，...，M}，并且由模型生成的候选边界框集合为C = {b^j | j = 1，...，N}。

