---
layout: post
title: "一键部署Huggingface模型工具Gradio"
date:   2023-03-09
tags: [model_deploy]
comments: true
author: wshzd
---

使用过Huggingface的开发者，应该见过如下的界面，方便用户在文本框直接输入query，然后使用模型来预测输出答案

![图片](https://mmbiz.qpic.cn/mmbiz_png/N5aX12H1SickALjS15V3zsPfgwR3CicJ8csvTBXqobytGMThhtXxnIoIjZgvtCxibW5zsV4z7KTfRSgCQY2xJtgAg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

接下来，我们看一下实现步骤（使用的部署工具是python包gradio）：

Note：部署工具可以在python任意的IDE运行，比如pycharm、jupyter notebook以及Google Colab，下面以Google Colab为例进行展示GPT-2模型的部署步骤：

**Step1**：首先就是安装python包

```
!pip install gradio
!pip install transformers
```

**Step2**：为了便于理解，我们首先实现一个输入文本，输出文本的demo，代码如下：

```
# Let’s get started with a simple “Hello World”
import gradio as gr

def greet(name):    
	return "Hello " + name

# We instantiate the Textbox class
demo = gr.Interface(fn=greet, inputs="text", outputs="text")

demo.launch()
```

运行上述脚本，会生成如下界面：

![图片](https://mmbiz.qpic.cn/mmbiz_png/N5aX12H1SickALjS15V3zsPfgwR3CicJ8cjwDDgHSUnibJvneecWWicmaOGo8TLNXhZK1nmItdsGjW8vyrpH2OvAug/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

用户在name文本框中输入文本“x”，会在output文本框中生成“Hello x”文本内容

![图片](https://mmbiz.qpic.cn/mmbiz_png/N5aX12H1SickALjS15V3zsPfgwR3CicJ8cmPoUSrjibe0WU2B3qIInzpOmziaibdKufWdSwCbfakicLBQtfGDxRwVlicw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

**Step3**：当然以上demo也是可以进行润色的

```
# Upgrade “Hello World” 
import gradio as gr

def greet(name):    
	return "Hello " + name

# We instantiate the Textbox class
textbox_input = gr.Textbox(label="Type your name here:", placeholder="John Doe", lines=2)
textbox2_output = gr.Textbox(label="View output result here:", placeholder="Arron", lines=2)
demo = gr.Interface(fn=greet, inputs=textbox_input, outputs=textbox2_output)

demo.launch(debug=True, share=True)
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/N5aX12H1SickALjS15V3zsPfgwR3CicJ8c0kK9GD6xicL5nzGwwIfuX4VHs3zYR4z17y6pibYRu7yDhrP1CaNklkOg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

**Step4**：我们在了解基于的操作之后，我们以一个GPT-2模型为例说明算法模型是如何部署的

```
# Let’s now build a simple interface that allows you to demo a text-generation model like GPT-2.
from transformers import pipeline

model = pipeline("text-generation")

def predict(prompt):    
	completion = model(prompt)[0]["generated_text"]    
	return completion
```

运行上述代码，会下载GPT-2模型

**Step5**：我们先来看一下GPT-2模型的预测结果

```
# Predict prompt based on GPT-2
predict("My favorite programming language is")
```

My favorite programming language is Python. It comes in both standard and custom forms, and each supports different types of manipulation. Here's a look at three of the most popular forms in Python, but you're welcome to share your own.

What

**Step6**：我们使用gradio部署模型来进行预测，由于GPT-2模型的生成是随机的，因此两次结果不一定会完全一致

```
# Using gradio API to visual the GPT-2 model
import gradio as gr

gr.Interface(fn=predict, inputs="text", outputs="text").launch()
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/N5aX12H1SickALjS15V3zsPfgwR3CicJ8c9nQs64MGcTCiblM9QfLwCJha0lJ5oz63nDbFEibwpVujOibtS3093NFHg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

**gradio的主要API介绍：**

`Interface(fn, inputs, outputs, ...)`

These parameters are:

- `fn`: the prediction function that is wrapped by the Gradio interface. This function can take one or more parameters and return one or more values
- `inputs`: the input component type(s). Gradio provides many pre-built components such as`"image"` or `"mic"`.
- `outputs`: the output component type(s). Again, Gradio provides many pre-built components e.g. `"image"` or `"label"`.

You can customize the behavior of `launch()` through different parameters:

- `inline` - whether to display the interface inline on Python notebooks.
- `inbrowser` - whether to automatically launch the interface in a new tab on the default browser.
- `share` - whether to create a publicly shareable link from your computer for the interface. Kind of like a Google Drive link!

完整的API介绍，请参考：https://gradio.app/docs/

