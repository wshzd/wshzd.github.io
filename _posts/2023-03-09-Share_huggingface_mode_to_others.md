---
layout: post
title: "共享自己的Huggingface模型给其他人使用"
date:   2023-03-09
tags: [PTM]
comments: true
author: wshzd
---

在Huggingface开发或者上传的模型，怎么分享给别人使用呢？其实是比较简单的，使用python gradio即可完成。

首先需要安装所需要的包

```
!pip install gradio
!pip install transformers
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/N5aX12H1SickALjS15V3zsPfgwR3CicJ8c7XTJ31DXBkLp1XVObhicssy9IsliaDyXW6vVxicSAlA9Ndzm7cia7mohKg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

实现上图所示结果，代码比较简单，如下所示：

```
from transformers import pipeline

model = pipeline("text-generation")

def predict(prompt):    
	completion = model(prompt)[0]["generated_text"]    
	return completion
```

**临时链接：**

如果想给别人发个临时链接，可以使用如下代码实现，一般是72小时之内有效

```
import gradio as gr

title = "Ask Rick a Question"
description = """
The bot was trained to answer questions based on Rick and Morty dialogues. Ask Rick anything!<img src="https://huggingface.co/spaces/course-demos/Rick_and_Morty_QA/resolve/main/rick.png" width=200px>
"""

article = "Check out [the original Rick and Morty Bot]
(https://huggingface.co/spaces/kingabzpro/Rick_and_Morty_Bot) that this demo is based off of."
gr.Interface(    
	fn=predict,    
	inputs="textbox",    
	outputs="text",    
	title=title,    
	description=description,    
	article=article,    
	examples=[["What are you doing?"], 
	["Where should we time travel to?"]],
).launch(share=True)
```

这里解释一下Interface接口参数的含义

To add additional content to your demo, the Interface class supports some optional parameters:  

- **title**: you can give a title to your demo, which appears above the input and output components.  
- **description**: you can give a description (in text, Markdown, or HTML) for the interface, which appears above the input and output components and below the title.  
- **article**: you can also write an expanded article (in text, Markdown, or HTML) explaining the interface. If provided, it appears below the input and output components.  
- **theme**: don’t like the default colors? Set the theme to use one of default, huggingface, grass, peach. You can also add the dark- prefix, e.g.darkpeach for dark theme (or just dark for the default dark theme).  
- **examples**: to make your demo way easier to use, you can provide some example inputs for the function. These appear below the UI components and can be used to populate the interface. These should be provided as a nested list, in which the outer list consists of samples and each inner list consists of an input corresponding to each input component.  
- **live**: if you want to make your demo “live”, meaning that your model reruns every time the input changes, you can set live=True. This makes sense to use with quick models (we’ll see an example at the end of this section) Using the options above, we end up with a more complete interface. Run the code below so you can chat with Rick and Morty:  

**永久链接：**

永久使用需要在Hugging Face Spaces中上传代码，参考链接：https://huggingface.co/blog/gradio-spaces

**从Hugging Face两个来源加载模型**

Interface接口的模型也不是必须要提前定义好，也可以从Hugging Face Hub和Hugging Face Spaces中直接加载，方法如下：

```
# load model from Huggingface Hub
gr.Interface.load(    
	"huggingface/EleutherAI/gpt-j-6B",    
	inputs=gr.Textbox(lines=5, label="Input Text"),    
	title=title,    
	description=description,    
	article=article,    
	examples=examples,    
	enable_queue=True,
).launch()

# load model from Huggingface spaces
gr.Interface.load(
	"spaces/abidlabs/remove-bg", inputs="webcam", title="Remove your webcam background!"
).launch()
```