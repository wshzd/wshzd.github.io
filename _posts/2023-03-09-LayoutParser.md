---
layout: post
title: "超越OCR的富文档内容解析神器LayoutParser"
date:   2023-03-09
tags: [ChatGPT]
comments: true
author: wshzd
---

上篇介绍了百度ERNIE-Layout，其中关键部分就使用了LayoutParser工具，感兴趣的也可以先阅读：[文档智能之ERNIE-Layout](http://mp.weixin.qq.com/s?__biz=Mzg3NDIyMzI0Mw==&mid=2247485685&idx=1&sn=db920ba59c7e23b64593d625c86b535c&chksm=ced54b11f9a2c20724e3dab47e75c81834b32a6ca8ec7966802da429100fc0b523bc5fa17a0b&scene=21#wechat_redirect)

论文题目：《A unified toolkit for Deep Learning Based Document Image Analysis》

论文链接：https://arxiv.org/abs/2103.15348

论文官方网站：https://layout-parser.github.io/

论文开源项目：https://github.com/Layout-Parser/layout-parser

![图片](https://mmbiz.qpic.cn/mmbiz_jpg/N5aX12H1SickYdFoTpbAUeftQOHm1QnIobF8Z8nedUSsOyia2SVA8ibzRib9V3AutszZOjicuicw3SR7re4lpMsiafibXA/640?wx_fmt=jpeg&wxfrom=5&wx_lazy=1&wx_co=1)

上图是来自一篇论文，从图中可以看出结构是非常复杂的，既有图片，又有文本，而且文本还是两列形式，传统的OCR是按行进行识别的，识别结果可想而知是混乱的，而本文提出的LayoutParser是借助于目标检测模型来提取重要的内容patch，从而避免了两列内容按行扫描的混乱结果。BUT，LayoutParser的贡献不仅如此，下面来解密一下吧

**LayoutParser核心贡献**：

- 提供了基于深度学习模型的layout检测、字符识别以及其他document image analysis (DIA) 任务的toolkit工具包；
- 支持非常丰富的预训练模型；
- 支持文本图像数据的标注以及模型的客户化；
- 可以分享和贡献模型的社区；

**LayoutParser架构图**：

![图片](https://mmbiz.qpic.cn/mmbiz_png/N5aX12H1SickYdFoTpbAUeftQOHm1QnIohm78kn1kqMD9ID93sW4bkGe8Kv5NAwve3SmzkDS34T3ZMDz2dia2yiag/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

主要包括五个组件，分别是：layout detection models、layout data structures、OCR module、visualization and storage和layout data annotation and model training。

**Layout detection models**

Layout Detection模块主要是对文档图片进行目标检测识别，比如使用Faster R-CNN、Mask R-CNN。LayoutParser是基于Detectron2提供最小的接口，下面是调用的示例代码：

```
import layoutparser as lp
image = cv2. imread (" image_file ") # load images
model = lp. Detectron2LayoutModel ("lp :// PubLayNet / faster_rcnn_R_50_FPN_3x / config ")
layout = model . detect ( image )
```

模型与数据集有适配问题，加载模型的格式应该采用如下格式：lp://<dataset-name>/<model-architecture-name>

目前已经预支持9个预训练模型和5个数据集（注意：如果不满足需求，可以上传用户自定义模型），如下表所示：

![图片](https://mmbiz.qpic.cn/mmbiz_png/N5aX12H1SickYdFoTpbAUeftQOHm1QnIoWFFCfvNm2BYyD8B9VW9RQK3ICqZeFt5k2jPG72Z8AeNJib6Vzr9BbZQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

**Layout data structures**

LayoutParser还提供了三种不同粒度的Layout数据类型，分别是：Coordinate、TextBlock和Layout，他们的关系如下图所示：

![图片](https://mmbiz.qpic.cn/mmbiz_png/N5aX12H1SickYdFoTpbAUeftQOHm1QnIo5cDkRfpRJx5zicREfuloSphg4l2Zwq4zDx9XZ8iaQD2E14jib5DpSIfrQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

**Coordinate**是Layout的基本数据类型，支持三种类型，分别是：Interval、Rectangle和Quadrilateral。Interval和Rectangle分别使用2、4个参数来定位1D和2D区域；Quadrilateral支持一些复杂的区域提取，参数有4个顶点和8个自由度；

**TextBlock**存储每个Layout的位置和其他特性，通过指定parent字段可以指定Layout的阅读顺序；

**Layout**包括TextBlock的列表，也支持处理一个batch的数据，Layout同样可以嵌套使用。

以上三种Layout数据类型支持的运算操作如下表所示：

![图片](https://mmbiz.qpic.cn/mmbiz_png/N5aX12H1SickYdFoTpbAUeftQOHm1QnIopulmErehL7ia8I5xCEnzxKibCmS91nibCxiczd1P4N17WYLx7Dpazicn31w/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

**OCR module**

LayoutParser为常见的OCR工具提供了统一的接口，示例代码如下：

```
ocr_agent = lp.TesseractAgent()
# Can be easily switched to other OCR software
tokens = ocr_agent.detect(image)
```

**Visualization and storage**

LayoutParser支持导出json、CSV以及XML数据格式；支持导入模型训练的COCO、网页数据。当然也可以实时看到OCR的识别结果，如下图所示：

![图片](https://mmbiz.qpic.cn/mmbiz_png/N5aX12H1SickYdFoTpbAUeftQOHm1QnIoJBxDFSkpyRngOF21FdwickqNFWUicQovRUzgreVWUG2IicRswjVW0GoZg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

**Layout data annotation and model training**

LayoutParser集成了主动学习工具，用户只需要标注图像中的部分目标，其他目标就会自动被标注，可以大大减少用户的标注时间

个人使用感受：在一些结构复杂的文本图像上提取效果比使用OCR工具要好很多。