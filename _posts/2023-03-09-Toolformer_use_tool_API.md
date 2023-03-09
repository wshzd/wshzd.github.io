---
layout: post
title: "语言模型Toolformer可以学会使用其他工具API"
date:   2023-03-09
tags: [ChatGPT]
comments: true
author: wshzd
---

![图片](https://mmbiz.qpic.cn/mmbiz_png/N5aX12H1SickJZdXZgu9qj0YqRWjAg379mKo1nZkib1KnThZXD6NA5SXFxqIAic18icgBZHGEJicBvyiaMrceAL2cUSQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

论文地址：https://arxiv.org/abs/2302.04761

Toolformer是一个自监督的语言模型，在不牺牲其语言建模能力的情况下提高其在下游任务中的表现。

要点:

1. 提出一种叫做 Toolformer 的新语言模型，可以通过简单的 API 以自监督方式学习使用外部工具；
2. Toolformer 在各种下游任务中提高了零样本性能，可以与较大的模型竞争，而不牺牲其核心语言建模能力；
3. 展示了 Toolformer 可以学习使用一系列的工具，包括一个计算器、一个问答系统、两个不同的搜索引擎、一个翻译系统和一个日历。

语言模型(LM)在解决新任务方面表现出非凡的能力，特别是在规模以上。矛盾的是，它们在基本功能方面很吃力，比如算术或事实查询，而在这些方面，更简单、更小的模型却很出色。本文展示了 LM 可以通过简单的 API 教自己使用外部工具，并实现两个世界的最佳效果。本文提出 Toolformer，一种经过训练的模型，可以决定调用哪些 API，何时调用，传递哪些参数，以及如何将结果最好地纳入未来的标记预测中，以一种自监督方式完成，只需要对每个 API 进行少量的演示即可。本文结合了一系列的工具，包括一个计算器、一个Q/A系统、两个不同的搜索引擎、一个翻译系统和一个日历。Toolformer在各种下游任务中实现了大幅提高的零样本性能，通常与更大的模型竞争，而不牺牲其核心语言建模能力。

学习的Pipeline如下图所示：

![图片](https://mmbiz.qpic.cn/mmbiz_png/N5aX12H1SickJZdXZgu9qj0YqRWjAg379dhEIg6UjBkKmfrccYrbkaxIvwKiaKEXD0QiaU7kDhRtyf90sgjLXZJicg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)