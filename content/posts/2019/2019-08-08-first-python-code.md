---
title: 用Python‘画’小猪佩奇
author: Boylad
date: '2019-05-08'
slug: first-coding
categories:
  - '2019'
tags:
  - Coding
layout: posts
Description: ''
---


四月份的时候，学院举办的数学文化节中一项活动是通过编程的方法画一只小猪佩奇。当时我初学Python没多久，知道有一个<a href="https://docs.python.org/zh-cn/3/library/turtle.html" target="_blank">`turtle`</a>库可以控制笔触在画布上移动而形成图案。于是我就想着以此来‘画’一只小猪佩奇，作为初学Python编程的一次尝试。

<figure align="center">
  <img src="https://gitee.com/boylad/images-for-blog/raw/master/before_2020/Peppa_Pig.jpg" height="300" width="300" title = '小猪佩奇'/>
</figure>

对于一个复杂且无规律可循的图案，通过代码控制笔触是匪夷所思的。我意识到可以将每个像素点画出来，这样只需要不断画直线就可以了，但这意味着低效率。为了提高效率，我尝试了<a href="https://computing.llnl.gov/tutorials/parallel_comp/" target="_blank">并行计算</a>——同时画多条直线，然而我没能成功。从理论上讲这是完全可以实现的，可能是我对Python还不够熟悉吧。

以下的Python代码要做的事情非常简单：先获取一张小猪佩奇图片，将其像素点依次储存到一个名为array的列表中，列表中的每个元素为一个三元（R、G、B）元组；然后用Turtle依次遍历此列表中每个元素，Turtle的颜色亦遍历列表中每个元素，并用循环语句控制好换行。


```python
import urllib.request
from PIL import Image
import turtle
import os

url = "https://raw.githubusercontent.com/Guankuiliu/"\
       "image/master/Figure2018/git/Peppa_Pig.jpg"
name = "Peppa_Pig.jpg"
conn = urllib.request.urlopen(url)
f = open(name,'wb')
f.write(conn.read())
f.close()

im = Image.open("Peppa_Pig.jpg")
im = im.resize((180, 180))
im = im.rotate(90)
os.remove("Peppa_Pig.jpg")

width = im.size[0]
height = im.size[1]
im = im.convert('RGB')
array = []
for x in range(width):
    for y in range(height):
        r, g, b = im.getpixel((x,y))
        rgb = (r/255, g/255, b/255)
        array.append(rgb)

turtle.screensize(canvwidth = width, canvheight = height, bg = 'green')
turtle.setup(width = 0.8, height = 0.8)
t = turtle.Turtle()
t.shape('turtle')
t.color('black')
t.speed(10)
t.penup()
t.goto(-500,250)
t.pensize(1)
i_1 = 0

for j in range(width):
    while i_1<height*(j + 1):
        t.pendown()
        t.color(array[i_1])
        t.pencolor(array[i_1])
        t.forward(1)
        i_1  += 1
    i_1 += height
    t.right(90)
    t.goto(-500+height,250-j-1)
    t.right(90)
             
    while i_1 > height*(j + 1):
        t.pendown()
        t.color(array[i_1])
        t.pencolor(array[i_1])
        t.forward(1)
        i_1  -= 1
    t.left(90)
    t.goto(-500,250-j-2)
    t.left(90)
```

因为效率问题，我将以上代码运行结果用<a href="https://www.xbox.com/en-US/" target="_blank">XBox</a>录屏后又用PS进行了倍速处理，32倍速的效果如下：

<center>
<iframe  width="100%"  height="300" allow="autoplay; encrypted-media" src="//player.bilibili.com/player.html?aid=62765143&cid=109050637&page=1?rel=0&amp;autoplay=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>
</center>
