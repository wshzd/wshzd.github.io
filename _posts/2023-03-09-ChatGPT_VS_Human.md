---
layout: post
title: "ChatGPT离人类专家还有多远？"
date:   2023-03-09
tags: [ChatGPT]
comments: true
author: wshzd
---

ChatGPT的推出在学界和业界引起了广泛的关注和讨论，人们对ChatGPT的强大能力产生了极大的兴趣，同时也引起了社会对于AIGC（AI-generated content）潜在风险的担忧。

在这样的背景下，一群来自**上海财经大学**、**哈尔滨工业大学（深圳）**、**北京语言大学**、**西安电子科技大学**、**加拿大皇后大学**以及**万德信息技术有限公司**的博士生或工程师，共同组建了一个研究团队，展开了一项名为「**ChatGPT对比&检测**」的研究项目。项目组成立于2022年12月9日，距离ChatGPT推出仅仅过去了10天，可能是**国际上最早**展开ChatGPT与人类对比分析研究和开发ChatGPT内容检测器的团队。

今年1月19日，初步研究成果发布于Arxiv，并开源了相关的数据集和模型。

- 论文标题：**How Close is ChatGPT to Human Experts? Comparison Corpus, Evaluation, and Detection**
- 论文链接：https://arxiv.org/pdf/2301.07597.pdf
- 项目主页：https://github.com/Hello-SimpleAI/chatgpt-comparison-detection

![图片](https://mmbiz.qpic.cn/mmbiz_png/N5aX12H1SickuNCmjicNDp6s1cZb92EHzGJC5lIUnEce0t56AwIDQCHVpRPau0vnoMMBj0zqIUZntdlRmYibITYag/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

**主要贡献：**

- 收集了近4万条和问题和对应的人类以及ChatGPT的回答，包含了开放域、计算机、金融、医疗、法律、心理等多个领域，并且兼顾了中文和英文。称这个数据集为 Human ChatGPT Comparison Corpus (HC3)，这个数据集可用于大型语言模型（LLMs）相关的研究，尤其是研究人类跟LLMs的差异；
- 进行了大量的人工测评和语言学分析，发现了很多有意思的结论，这些发现有助于我们思考未来大型语言模型应走向何方；
- 根据HC3数据集和分析，开发了多种ChatGPT内容检测器，应对不同的场景。这些检测器可用于UGC（User-generated Content）平台的监管，提升内容的可靠性、安全性；

**数据集：**

![图片](https://mmbiz.qpic.cn/mmbiz_png/N5aX12H1SickuNCmjicNDp6s1cZb92EHzGuIBy345nycEjNo9O2a2osOSYWDLCnEAfQEia6iblbbGDxwN0Rw5kYhmw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

数据都以JSON字典的格式进行存储，示例如下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/N5aX12H1SickuNCmjicNDp6s1cZb92EHzGWyO4m2giaVic0M5zZyAoEj3993ic1JkkoaJmgDVtxtuiaWvh5X5l2lHcUQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

**主要结论如下：**

**人工评测**：

![图片](https://mmbiz.qpic.cn/mmbiz_png/N5aX12H1SickuNCmjicNDp6s1cZb92EHzGrFmPXSrOByVMDJ8I2Y6BuoD2OiaicstCl91Ric0GvQQwzZoZxiaYdQkK4Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

结果如上图所示，可以得出如下结论：

- **专家来判断，那简直是太容易了**，给专家的对比测试，很多数据集直接100%正确，这说明经常使用ChatGPT的人，已经深谙它的套路了，尤其跟人的回答可以直接对比的情况下；
- 对于专家，**如果只给一条文本，这时准确率就下降了**，差不多降低了10%，但依然很高；
- **一旦一个人不知道ChatGPT，那可就麻烦大了**，基本一半的情况下都可能猜错，有些数据集，甚至准确率可能低到不到20%（有可能是那个数据集的人类回答太差了，让我们的志愿者觉得这个像机器，但这也说明，普通大众对于机器文本生成的水平，还了解很有限）。
- **“有用性测试”，即图中的helpfulness，微微超过一半的情况，ChatGPT的回答被认为是更有用的**，比方金融问题中，ChatGPT的回答一般十分专业、详细，甚至能让人学到很多知识。但是对于有些领域则不太行，比如医疗领域，这可能是涉及的知识过于专业，在ChatGPT训练语料中并不很多，导致ChatGPT回答有时候过于模糊，而人类专家则直击痛点，所以被认为更有帮助。

**语言学分析**：

**词汇特征：**

![图片](https://mmbiz.qpic.cn/mmbiz_png/N5aX12H1SickuNCmjicNDp6s1cZb92EHzG89d10dRZDUWA6uvM1QLzqTBarSwgI21qwwPR28LfrHhxks7mDqeNBQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

**结论如下**：

- 人类的回答一般更短，但却说了更丰富的词汇。

**情感分析：**

![图片](https://mmbiz.qpic.cn/mmbiz_png/N5aX12H1SickuNCmjicNDp6s1cZb92EHzGZQONpQLlrHKRENnyQu6AUfVlNCdzASgzxUWZgVdicNntqiaHTzE8pFtg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

**结论如下**：

- ChatGPT确实更加理性甚至“正能量”一点。

  

**ChatGPT检测器**

- 开发了三种类型的检测器，用于不同的场景：QA version / 问答版: 判断某个**问题的回答**是否由ChatGPT生成，使用基于PTM（RoBERTa）的分类器来开发;
- Sinlge-text version / 独立文本版:  判断**单条文本**是否由ChatGPT生成，使用基于PTM（RoBERTa）的分类器来开发;
- Linguistic version / 语言学版:  判断**单条文本**是否由ChatGPT生成，使用基于语言学特征的模型（GLTR）来开发;

![图片](https://mmbiz.qpic.cn/mmbiz_png/N5aX12H1SickuNCmjicNDp6s1cZb92EHzGpJnibm5AAskWHEFkVbyCmDStAkYDcv3WicLDO5q0zQib82wgQhSxse10Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

**结论如下**：

- 基于深度学习的方法，效果明显优于机器学习的方法；
- 在句子层面进行检测的难度明显高于全文层面；
- 对指示词进行过滤，可以帮助全文提升检测性能，但是却可能影响句子层面的检测效果

