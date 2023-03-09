---
layout: post
title: "ChatGPT可以作为一个翻译器吗？"
date:   2023-03-09
tags: [ChatGPT]
comments: true
author: wshzd
---

![图片](https://mmbiz.qpic.cn/mmbiz_png/N5aX12H1SicmUcKOIgAiaJdFzk5LWyRKaOicL14SuNZSDYhTgaicib9XrEKnHOsIn55RoySRBbSjjAjTu62M50oicCLg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

论文地址：https://arxiv.org/abs/2301.08745.pdf

**背景**

自从OpenAI2022年11月30日发布ChatGPT以来，基本上把NLP所有任务大统一了，那么在机器翻译的表现到底如何呢？腾讯AI Lab在**翻译Prompt**、**多语言翻译**以及**翻译鲁棒性**三方面做了一些实验，并且与**Google Translate（133种语言），** **DeepL Translate（29种语言）**和**Tencent TranSmart（16种语言）**三款商业翻译软件进行了对比。

**核心结论**

1、在高资源语言上的翻译效果和主流商业翻译软件(Google翻译，deepl等)相当；

2.在低资源语言上的翻译效果差强人意，通过pivot prompting方法可以提升效果；

3.在翻译的鲁邦稳定性上落后于主流商业翻译软件；

4.ChatGPT在口语翻译上是一个潜在的好工具；

**ChatGPT机器翻译评估**

**评估数据**

![图片](https://mmbiz.qpic.cn/mmbiz_png/N5aX12H1SicmUcKOIgAiaJdFzk5LWyRKaOtWVdicBRWZ1M6DlRiaYBQB1ZC6Y38ZaiaJg2ibQeHVX7ibB4WWqJuPickCeQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

由于翻译数据需要手动去和ChatGPT交互得到，比较耗时，因此作者**每个种类的数据集都随机采样了50个样本**进行评估

**评估指标**

BLEU、ChrF++、TER，这三种评估指标可以在如下链接找到实现方案：https://github.com/mjpost/sacrebleu

**翻译的Prompts**

首先作者使用如下Prompt在ChatGPT上生成翻译需要的Prompts

![图片](https://mmbiz.qpic.cn/mmbiz_png/N5aX12H1SicmUcKOIgAiaJdFzk5LWyRKaOccVj6bUxbE31lMOBcQ3SDpm5cdwIBKyHPGoyYPjfXvz14Wo1vEJJww/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

如上图所示，发现生成的Prompt是合理的，但基本都很类似的，作者重新进行了归类，如下表所示：

![图片](https://mmbiz.qpic.cn/mmbiz_png/N5aX12H1SicmUcKOIgAiaJdFzk5LWyRKaOqa1j7I1qtPGaeO4ONXBLckrESAmaVDt4YyqxibaZ3FEYeB3o6SJC6fw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

**[SRC]**表示源语言，**[TGT]**表示目标语言。由于在Figture1中生成的Prompt中都带有“**引号**”，作者在Tp2中增加了去掉生成“**引号**”的Prompt，不过这样偶尔导致生成不稳定。

![图片](https://mmbiz.qpic.cn/mmbiz_png/N5aX12H1SicmUcKOIgAiaJdFzk5LWyRKaOoX2UVLkIZsu8hq8xt7iaCVnzaANe0aBN53UEw3kq89PzApNHhU2I8Ow/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

从上述三种翻译Prompt的实验来看，Tp3是最佳的，因此后面的实验都使用Tp3 Prompt。

**翻译的语言**

作者使用了四种语言进行两两之间进行翻译，以BLEU为平均指标，结果如下图所示：

![图片](https://mmbiz.qpic.cn/mmbiz_png/N5aX12H1SicmUcKOIgAiaJdFzk5LWyRKaO0dMhjiavmGdXwSXdv6zxwnoTON4elwxB4pa5Ihf9rB9sOrqlwNsHGZQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

从上图可以看出，在**低资源的罗马语言到英语**的效果要比**高资源的德文到英语**的翻译差，所谓高资源，低资源是从他们的翻译语料多少来定的。

一般来说，低资源或者不同语种直接的翻译，传统商业软件是比较难的，而ChatGPT一个模型解决多任务的能力可以通过高资源的语料和NLP其他任务弥补这种不足。

为了解决不同语种的翻译，提出了一种叫Pivot Prompting的方法，具体做法就是先把一种语言翻译成中间语言，然后再翻译成目标语言，一般中间语言默认是英语。形式如下：Please provide the [PIV] translation first and then the [TGT] translation for these sentences one by one:

使用这种方法，Tp3可以修改成如下形式：

![图片](https://mmbiz.qpic.cn/mmbiz_png/N5aX12H1SicmUcKOIgAiaJdFzk5LWyRKaOXiaBKhkrCsybJIgcXr24SrPqt1kht7wN8R3KJLOqicfKjneRzY7ib3Ndg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

使用Pivot Prompting方法，并且对低资源语言翻译进行BLEU评估，结果如下Table 5所示。

![图片](https://mmbiz.qpic.cn/mmbiz_png/N5aX12H1SicmUcKOIgAiaJdFzk5LWyRKaOK0cwYU6em0L7ibaHlgPic8QLicTGQeoAMDy2IyzY8wxvibia5qLa9xD1J6Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

**ChatGPT在翻译任务的鲁棒性**

![图片](https://mmbiz.qpic.cn/mmbiz_png/N5aX12H1SicmUcKOIgAiaJdFzk5LWyRKaOvyLUfQuF82daFJibY8e6eYO6JGjqsapzhxFcOocvFiaSDwFic0aXLL8kA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

从Table 6可以看出，ChatGPT在这些翻译数据上不如商业软件Google Translate 和 DeepL Translate好。
下面看一些具体的case

![图片](https://mmbiz.qpic.cn/mmbiz_png/N5aX12H1SicmUcKOIgAiaJdFzk5LWyRKaOlk5yE55YX7DKQ5GdTHrg67prxxWwI3DZ4PrT2SvibtHP8qX1qdd0Sicg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

**参考文献：**

[1] https://github.com/wxjiao/Is-ChatGPT-A-Good-Translator
[2] https://translate.google.com
[3] https://www.deepl.com/translator
[4] https://transmart.qq.com/zh-CN/index
[5] https://github.com/facebookresearch/flores
[6]https://github.com/hsing-wang/WMT2020_BioMedical/tree/master/Bio-18-19-testset 
[7] https://github.com/mjpost/sacrebleu
