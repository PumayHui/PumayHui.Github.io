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

## 1. 什么是“end to end”？

- 指的是：输入是原始数据，输出是最后的结果。原来输入端不是直接的原始数据，而是在原始数据中提取的特征，这一点在图像问题上尤为突出，因为图像像素数太多，数据维度高，会产生维度灾难，所以原来一个思路是手工提取图像的一些关键特征，这实际就是就一个降维的过程。

- *通过缩减人工预处理和后续处理，尽可能使模型从原始输入到最终输出，给模型更多可以根据数据自动调节的空间，增加模型的整体契合度。* 

- 可以理解为end-to-end训练只用一个阶段，如：Fast R-CNN，Faster R-CNN；而R-CNN、SPP训练需要分为多个阶段：微调网络+训练SVM+训练边框回归器。

## 2. 目的

将图片分成网格，用LSTM在每个网格中单独预测人头，产生序列化的输出结果。

![](/img/in-post/2018-03-06-headCounting/An overview of model .png)

整体流程如上图：
1. 首先通过卷积架构（Christian Szegedy, Wei Liu, Yangqing Jia, Pierre Sermanet, Scott Reed, Dragomir Anguelov,Dumitru Erhan, Vincent Vanhoucke, and Andrew Rabinovich. Going deeper with convolutions.CoRR, abs/1409.4842, 2014.）将图像编码为高维描述符；
2. 然后将该特征解码为一组边界框，作为预测可变长度输出的核心机制，建立一个LSTM单元的循环网络；
3. 在整个图像的跨越区域将其转换成具有1024个维度特征的网格描述符，这1024维向量汇总了区域的内容并携带了关于对象位置的丰富信息；
4. LSTM从该信息源获取并且在区域的解码中充当控制器，每一步，LSTM输出新的边界框和对应的置信度，即在该位置处将发现先前未检测到的人。这些边界框将按照置信度降序生成；
5. 当LSTM在具有高于预定阈值的置信度的区域中不能再找到另一个框时，就会产生停止符号；
6. 输出序列将被拼接为该区域中所有对象实例的最终结果。

# 二.loss function

一中介绍的架构，预测一组候选边界框以及与每个框相对应的置信度得分。

在每次重复训练时，LSTM输出一个对象边界框b = {b_pos，b_c}，其中b_pos =（b_x，b_y，b_w，b_h）∈R^4 是边界框的相对位置、宽度和高度，b_c∈[0, 1]是置信度的真值。

低于预定阈值（如0.5）的置信度值在evaluate时将被视为停止符号，较高的边界框置信度b_c表明该边界框更可能对应于正确值。

将相应的标准真值边界框集合表示为G = {b^i|i = 1，...，M}，由模型生成的候选边界框集合为C = {b^j|j = 1，...，N}。

![](/img/in-post/2018-03-06-headCounting/location.png)

如上图中的例子表示具有四个生成假设的检测器，每个假设由其预测步骤编号。注意典型的检测错误，如假阳性（假设3），不精确的定位（假设1）和同一真值实例的多重预测（假设1和2）。不同的错误需要不同种类的反馈。在假设1的情况下，边框位置必须被微调。相反，假设3是假阳性，模型应当通过设置低置信度得分来丢弃这个预测。假设2是对已经由假设1预测过的目标的第二次预测，也应该被丢弃。

为了确定假设之间的关系，引入匹配算法为每个标准真值分配唯一的候选假设。该算法返回单射函数f：G→C，即 f(i) 是分配给标准真值假设i的候选假设的索引。

给定f，我们在集合G和C上定义损失函数：

![](/img/in-post/2018-03-06-headCounting/loss function.png)

其中l_pos为：

![](/img/in-post/2018-03-06-headCounting/l_pos.png)

是标准真值位置和候选假设之间的位移，l_c是候选框置信度的交叉熵损失，它将与标准真值进行匹配。该交叉熵损失的标签由y_j提供，它由匹配函数定义得到：

![](/img/in-post/2018-03-06-headCounting/yi.png)

其中α是置信度误差和定位误差之间的系数项。我们交叉验证设置α = 0.03。对于固定匹配，我们可以通过反向传播这个损失函数的梯度来更新网络。

作为一个原始基线，考虑一个基于标准真值边界框的固定顺序的简单匹配策略。通过图像位置从上到下和从左到右排序标准真值框。该固定顺序匹配序列化地将候选者分配给排好序的标准真值。将这个匹配函数称为“固定顺序”匹配，将其表示为f_fix，与其对应的损失函数表示为L_fix。

当两个假设都有效地和同一真值实例重叠时（例如，图中的假设1和2），我们优选匹配在预测序列中较早出现的假设。为了形式化这个概念，引入以下假设与标准真值的比较函数：

![](/img/in-post/2018-03-06-headCounting/compare function.png)

函数Δ：G×C→N×N×R 返回一个元组，其中d_ij是边界框位置之间的L1距离，r_j是LSTM输出的预测序列中的b_j的秩或索引，o_ij∈{ 0，1}是假设与标准真值实例不充分重叠的惩罚变量。这里，重叠标准要求候选者的中心要位于标准真值边界框的范围内。o_ij变量明确区分定位和检测错误。我们定义了一个由Δ产生的元组的词典顺序。也就是说，当评估两个假设中的哪一个将被分配给标准真值时，重叠是最重要的，随后是秩，然后再是细粒度定位。

给定方程2中的比较函数Δ的定义，通过匈牙利算法在多项式时间内找到C和G之间的最小成本二分匹配。匈牙利算法适用于具有明确定义的加法和成对比较运算的带边权的任何图。因此定义（+）作为元素相加，（<）作为词典比较。对于图中的例子，正确匹配假设1和4将花费（0, 5, 0.4），而匹配1和3将花费（1, 4, 2.3），匹配2和4将花费（0, 6, 0.2）。注意，用于检测重叠的第一项是如何适当地处理那些尽管具有低秩，但离标准真值差太远而不足以成为敏感匹配的假设的情况（如图3中的假设3的情况）。将这种匹配的相应损失称为匈牙利损失，并表示为L_hung。

# 三. procedure

1. Model training
- Caffe 
2. Initialization
- GoogLeNet weights baseline : OverFeat model(initialized with Imagenet pretraining)
3. Regularization
- dropout with probability 0.15 on the output of each LSTM
4. Stitching
- While  we  trained  our  algorithm  to  predict  bounding  boxes  on  64x64  pixel  regions, we apply our algorithm to full 480x640 images at test time.  To this end, we generate predictions from each region in a 15x20 grid of the image.  We then use a stitching algorithm to recursively merge in predictions from successive cells on the grid. 
