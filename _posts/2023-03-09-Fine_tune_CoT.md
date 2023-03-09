---
layout: post
title: "CoT应用在小模型：Fine-tune-CoT"
date:   2023-03-09
tags: [ChatGPT]
comments: true
author: wshzd
---

**Paper_Name**：《Large Language Models Are Reasoning Teachers》

**Paper_URL**: https://arxiv.org/pdf/2212.10071.pdf

## **背景**

最近的研究表明，通过包含思维链 (CoT) 推理或促使模型一步一步地思考的几个示例，可以在大语言模型 (LLM) 中激发复杂推理能力。

基于Prompt的CoT推理方法的一个主要缺点是它们依赖于数千亿参数的超大模型，由于巨大的计算需求和推理成本，这些模型无法在实际使用中大规模部署。

虽然有人试图通过显式推理步骤来微调小模型来解决这个问题，但需要大量的复杂的推理标注数据，而且通常还需要根据特定任务进行训练设置。

## **论文核心**

提出了 Fine-tune-CoT 方法，旨在利用非常大的语言模型 (LMs) 的CoT推理能力来教导小模型如何解决复杂任务，从实验来看，蒸馏出来的小模型在某些数据集上精度甚至能超过 teacher 大模型。

**Fine-tune-CoT的蒸馏实现方案**

应用现有的 zero-shot CoT 提示从非常大的教师模型中生成理据，并使用它们来微调较小的学生模型，基本步骤如下图所示：

![图片](https://mmbiz.qpic.cn/mmbiz_png/N5aX12H1SickuNCmjicNDp6s1cZb92EHzG8ImPsia5iargA9jSbXf78034PdK4r0w3kovu2ICRZ7SiamW9T4vBwwWAA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

**Step 1. Reasoning generation**

首先利用一个超大型教师模型为给定的任务根据提示生成多步推理解释(绿色)来解决复杂的问题(黄色)

生成的文本序列，包括提示符和生成，采用以下形式：“Q: . A: Let’s think step by step. <ˆri> Therefore, the answer is <ˆai>”；

**Step 2. Curation**

过滤上述生成的样本重新给fine-tunig阶段准备样本，具体是将教师模型的最终预测ˆai与 ground-truth 答案 ai 进行比较，挑选预测ˆai与 ai 一致的样本，再将(Si, ˆri，ˆai ) 重新打包成一个推理样本S’i = (pi, ci)，一个提示补全对；

**Step 3. Fine-tune**

使用与预训练期间相同的训练目标，即 autoregressive language modeling objective 或 next-token prediction，在OpenAI API上对一个小型预训练的学生模型进行fine-tuning

**Diverse reasoning**

为了使微调样本效率最大化，为每个训练样本生成多个推理解释，从而增加微调数据，具体而言，对于给定的样本 Si，采用随机采样策略，即T较大的温度采样，而不是采用贪婪解码的 Zero-shot-CoT 来获得单个解释-答案对 (ei, ai)，以获得 D 个不同的样本对。

**实验结果**

**OpenAI API模型列表，学生模型比非常大的教师模型小25 - 500倍**

![图片](https://mmbiz.qpic.cn/mmbiz_png/N5aX12H1SickuNCmjicNDp6s1cZb92EHzGcu5ibbLtvqmzEfTyibNGrg8OtnoRbF3l7JZFDaW6auawmiad8JrE82YKw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

**baseline对比**

![图片](https://mmbiz.qpic.cn/mmbiz_png/N5aX12H1SickuNCmjicNDp6s1cZb92EHzGSJLKqtfkzU4bzlkDUkI7wcGM5p5OMMqskcm7N5WOw7eKKkzwK74VDQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

Zero-shot-CoT：task-agnostic prompting baseline
finetuning：使用原始训练样本进行 finetune
Fine-tune-CoT：本文方法

**12 个数据集上的对比结果**

![图片](https://mmbiz.qpic.cn/mmbiz_png/N5aX12H1SickuNCmjicNDp6s1cZb92EHzGtuxZBG3KX3Hn4K7tOdTHFQlMDRDSIIPiav8gg3xKke9tV0hR8ViblzsQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

- Fine-tune-CoT 性能超过 Zero-shot-CoT
- 一些小模型效果超过了大模型：Shuffled Objects and Coin Flip
- Fine-tune vs Fine-tune-CoT：Date Understanding and Shuffle Objects 数据上 Fine-tune-CoT 比 Fine-tune 精度明显高（Fine-tune 精度与 Random 差不多），某些数据集上 Fine-tune 精度也会比 Fine-tune-CoT 高，不过 Fine-tune-CoT 性能随着模型大小显示出更可靠的缩放曲线，并在需要多个步骤的任务中显示出明显的优势

![图片](https://mmbiz.qpic.cn/mmbiz_png/N5aX12H1SickuNCmjicNDp6s1cZb92EHzGibvK2xVicJrwo3cu5FwGlUvWQrKtKMM1ZegbF9gjVZYdV8jXoicSlt62g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

- 多样化的推理理由可以泛化Fine-tune-CoT的性能

  **参考文献**：

  [1] https://blog.csdn.net/kebijuelun/article/details/128498072