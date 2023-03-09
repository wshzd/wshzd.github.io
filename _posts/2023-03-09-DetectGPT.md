---
layout: post
title: "DetectGPT（斯坦福大学）：利用概率曲率检测文本是否大模型生成"
date:   2023-03-09
tags: [ChatGPT]
comments: true
author: wshzd
---

论文标题：《DetectGPT: Zero-Shot Machine-Generated Text Detection using Probability Curvature》

论文链接：https://arxiv.org/abs/2301.11305

代码数据：https://ericmitchell.ai/detectgpt/

作者通过分析超大模型生成文章与人类文章的logp分布特点，发现大模型生成的文章倾向于在logp分布较小的区域，而人类写的则无明显倾向性，于是提出了DetectGPT来侦测文档是否有超大模型生成（比如ChatGPT）

![图片](https://mmbiz.qpic.cn/mmbiz_png/N5aX12H1SickYdFoTpbAUeftQOHm1QnIoBTkoj9CibibYgrXLfziatPly2bL9frhicO96nn987GKXd8de4AU3guqLRg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/N5aX12H1SickYdFoTpbAUeftQOHm1QnIoiaUkLwYAd82S5mElOGpIKvOQwEfHSgaP4Ck01qVefLRxKJIicclYia0sg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

从以下实验可以看出效果非常不错

![图片](https://mmbiz.qpic.cn/mmbiz_png/N5aX12H1SickYdFoTpbAUeftQOHm1QnIoLclmeJ3GeSVRojZgqYEYcc1ibEYBOpnFVJnCmcmORM47LW3nGvse28g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

下面是DetectGPT的实现原理和算法伪代码

![图片](https://mmbiz.qpic.cn/mmbiz_png/N5aX12H1SickuNCmjicNDp6s1cZb92EHzGLX933ZR1wK1XlA5JJC4WqPn2ENA3uj8yCH21F8rUHpDdKu3uFJEp5A/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/N5aX12H1SickuNCmjicNDp6s1cZb92EHzGRUS4IrgbUvOsticicI1Q7ElEVLlCZoibAcVErgLUvzmsv3n6Ivq8r68lA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)