---
layout: post
title: "Detecting LLM-Generated-Text综述"
date:   2023-03-09
tags: [LLM]
comments: true
author: wshzd
---

![图片](https://mmbiz.qpic.cn/mmbiz_png/N5aX12H1SicnfvJ7M7dafT0OTMarkLBibXicRcOqnnAb65uEqc8P5fqE3rrUVZw4wxjGibDuPPNTLvFUyCASP7UxBQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

 

- 论文地址：https://github.com/datamllab/The-Science-of-LLM-generated-Text-Detection
- 相关研究地址：https://github.com/datamllab/awsome-LLM-generated-text-detection/tree/main

大模型生成文本检测现有的方法大致可分为两类：**黑盒检测和白盒检测**。

![图片](https://mmbiz.qpic.cn/mmbiz_png/N5aX12H1SicnfvJ7M7dafT0OTMarkLBibXzbURO3sdibvlNMxwYvSpZiaSR89GcYDUyXoH4diczZ5BnhAODBibsGW6wA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

- 黑盒检测方法对大型语言模型通常只有 API 级别的访问权限。因此，这类方法依靠于收集人类和机器的文本样本来训练分类模型；
- 白盒检测，这类方法拥有对大型语言模型的所有访问权限，并且可以通过控制模型的生成行为或者在生成文本中加入水印（watermark）来对生成文本进行追踪和检测。

在实践中，黑盒检测器通常由第三方构建，例如 GPTZero，而白盒检测器通常由大型语言模型开发人员构建。

![图片](https://mmbiz.qpic.cn/mmbiz_png/N5aX12H1SicnfvJ7M7dafT0OTMarkLBibXTVqy5gmia2vfJzSalic4s5N5r9xY6nfEM0ic4iazeGDVdes5MISzuYDonA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

 