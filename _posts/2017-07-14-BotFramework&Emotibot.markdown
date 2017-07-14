---
layout:     post
title:      "BotFramework&Emotibot"
subtitle:   ""
date:       2017-07-14 17:00:00
author:     "Pumay"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - chabot
---


# BotFramework

## 1)概述

[https://dev.botframework.com/](https://dev.botframework.com/)

1. Bot Builder SDKs--Bot生成器，快速生成一个ASP.NET和Node.js的后端服务，提供了像Dialog、FormFlow帮你管理与用户的会话。

2. Bot Connector--Bot的Channel，帮你把你的服务快速发布到各个渠道，比如说Skype，Facebook Messager等等。这样用户就可以在Skype等Channel上使用你的服务了。

3. Bot Directory--Bot商店，在这里可以找到各个bot，你也可以把自己的Bot发布出来，从而大家都可以看到你的Bot。

![](http://images2015.cnblogs.com/blog/513999/201609/513999-20160906154434941-2085782362.png)

## 2)如何使用BotFramework构建bot？

**四种方式构建**

* **.NET**

> 用于开发使用Visual Studio和Windows机器人

> Bot Builder SDK利用C＃为.NET开发人员提供了创建机器人的方式。

> 需要安装Bot Builder SDK。但VS for Mac不支持.NET，未体验。

![](https://raw.githubusercontent.com/PumayHui/polly_python/master/DEEB1977-2FA4-45FB-9F39-1CEDA214CA18.png)

* **Node.​js**

> Node.js的Bot Builder SDK是开发机器人的框架。如Express＆restify，为JavaScript开发人员编写机器人提供方法。
> 
> 需要安装：Node.js、botbuilder、restify

* **Azure Bot Service**

> Azure Bot Service提供专为bot开发而设计的集成环境。
> 
> Azure Bot 服务可汇集 Microsoft Bot Framework 和 Azure Functions 的强大功能，实现快速智能机器人开发。生成、连接、部署和管理能够随时随地与用户自然交互的智能机器人。允许机器人按需缩放，仅为所用资源付费。
> 
> Azure服务分为[中国版](http://www.windowsazure.cn)和[国际版](	http://azure.microsoft.com)。其中中国版不提供bot service服务（见下图）。

> 个人用户使用国际版Azure，在国际版Azure的官网申请账号(需要国外的银行卡和电话)即可。

![](https://raw.githubusercontent.com/PumayHui/polly_python/master/C3839CA6-D01D-49FE-A638-E8C3481D6BFA.png)

![](https://raw.githubusercontent.com/PumayHui/polly_python/master/86E9180B-DD22-4CD0-905D-42FBC6160A43.png) 

可以创建四类bot:

<table>
<thead>
<tr>
<th>bot种类</th>
<th>description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Basic</td>
<td>创建一个使用对话框响应用户输入的机器人。</td>
</tr>
<tr>
<td>Form</td>
<td>创建一个机器人，通过使用FormFlow（在C＃中）或瀑布（在Node.js中）创建的引导对话来收集用户的输入。</td>
</tr>
<tr>
<td>Language understanding</td>
<td>创建一个使用自然语言模型（LUIS）理解用户意图的机器人。</td>
</tr>
<tr>
<td>Proactive</td>
<td>创建一个使用Azure功能来提醒用户事件的机器人。</td>
</tr>
<tr>
<td>Question and Answer</td>
<td>创建一个使用知识库来回答用户问题的机器人。</td>
</tr>
</tbody>
</table>

> 主要体验Language understanding即LUIS理解用户意图的功能。

> Azure分

* **REST**

> Bot connector使bot可以通过使用行业标准的REST和JSON通过HTTPS 与Bot Framework Portal中配置的通道交换消息。

## 3)如何调试bot?

> 三种方式：Bot Framework Emulator、Visual Studio Code、Azure Bot Service bot

## 4)如何部署bot?

> 从本地git仓库部署

> 从GitHub部署使用持续集成

> 从Visual Studio部署

## 5)资费

Bot Framework preview目前免费。此处列出搭建bot过程中用到的平台及调用的服务资费情况。

### a. Azure

先使用Microsoft Azure订阅，然后才能使用Azure Bot服务。订阅资费如下：

![](https://raw.githubusercontent.com/PumayHui/polly_python/master/fee.png)

> Azure Bot 服务可免费使用。只需为使用的资源付费。如LUIS。

### b. LUIS API

![](https://raw.githubusercontent.com/PumayHui/polly_python/master/LUIS.png)

# Emotibot
## 1)概述

[http://botfactory.emotibot.com/BF/platform.html?n=43](http://botfactory.emotibot.com/BF/platform.html?n=43)

竹间智能科技已经开发出了 **Bot Factory情感机器人工厂**， 一个为企业、个人提供情感机器人快速定制服务的平台。 通过Bot Factory情感机器人工厂，可以调用Emotibot智能对话机器人、语音情绪、机器视觉等AI技术，并对机器人进行形象定制、问答定制、知识定制、意图及多轮问答定制等。另外，定制的机器人自带机器自学习优化能力，也就是这个机器人会学习、会自我成长。

## 2)如何搭建bot?

![](https://pic3.zhimg.com/v2-a219d43d9409d8f127afc67d60a8951a_r.jpg)
## 3)优势
* 核心技术领先（技术很厉害）

> **语义理解**：基于上下文、用户意图及情感、对用户的记忆，让机器人做到跟人一样，能主动与人进行双向对话；

> **情感计算**：22种文字情感+4种声音情感+7种面部表情，让机器人真正地读懂、听懂、看懂用户；

> **机器视别**：人脸识别准确率高达98.42%，8大类服饰识别准确率80%以上，超过 22种人脸属性辨识，9种人脸情绪分析

* 各领域知识积累+高度定制（经验很丰富）

> **标准化平台**：竹间智能科技拥有长期的技术积累与各行业AI落地的经验，拥有电商、金融、IoT等领域丰富的定制知识库、功能及领域经验积累。
> 
>**可定制**：平台提供丰富易操作的可定制化服务，用户可以在管理界面中自行完成机器人形象定制，问答定制，知识定制，意图引擎定制等。

* 最自然的用户交互体验（最懂<span style="color: rgb(160, 160, 160); font-family: &#39;Helvetica Neue&#39;, Helvetica, &#39;Hiragino Sans GB&#39;, &#39;Microsoft YaHei&#39;, &#39;Apple Color Emoji&#39;, &#39;Emoji Symbols Font&#39;, &#39;Segoe UI Symbol&#39;, Arial, sans-serif; font-size: 14px;  text-decoration: line-through;">女人心</span>用户）

> **多模态情感计算下的情感交互**：通过语音，文字，图像的等能够深入理解用户所表达的情绪，根据内容和情绪多个维度进行分析，给出最合适的回答。让你的机器人从此不再冷冰冰。
> 
> **记忆能力支撑的多轮对话**：超过25个维度信息记忆，能够根据上下文的内容，综合用户的喜好/习惯和状态综合分析，给出最符合场景的回答，完成复杂的多伦交互。你的机器人不再是无厘头，它会像家人一样懂你。 
> 
> **强大自学习能力**：强大的自学习算法实时收集用户反馈，24x7进行算法训练优化。知错就改才是好孩子，好好学习才能天天向上。

* 体验全程0编程（全公司都很厉害）

>全图形化配置界面，支持Mobile+PC，操作人员无需任何编程基础，即可轻松地创建、定制与管理机器人。

## 4)资费

目前为免费试用。
