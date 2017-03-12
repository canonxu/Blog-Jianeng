---
layout: post
title: 一个简单的Blog爬虫程序
date: 2014-12-05
updated: 2017-03-11
categories:
- PYTHON学习
tags:
- Python
- 爬虫
---

### 背景
利用Python自带的urllib模块，对我自己博客的一篇文章[Pycon2014 @Shanghai](http://canonxu.com/2014/11/Pycon2014@Shanghai/)进行爬取，主要把图片爬下来。

### 代码
首先是读取并保存html文件，通过`urllib.urlopen()`方法连接到页面，`read()`方法读取页面，保存html文件，然后提取出html中的图片链接，利用正则表达式和re模块中的`re.findall()`提取出图片链接默认保存为list，最后，通过`rl.retrieve()`方法将图片直接下载，重命名。

<!-- more -->

代码如下:

```python
import urllib
import re
import os

os.path.join(r'E:\learn-python', r'blog_spider')
os.mkdir(r'E:\learn-python\blog_spider')

def gethtml(url):
    page = urllib.urlopen(url)
    html = page.read()
    return html

def getimg(html):
    img_re = re.compile(r'src="(.+?\.jpg)"')
    imglist = re.findall(img_re,html)
    return imglist

blog_html = gethtml('http://canonxu.com/2014/11/Pycon2014@Shanghai/')
with open(r'E:\learn-python\blog_spider\blog_html.html', 'w') as f:
   f.write(blog_html)

blog_imglist = getimg(blog_html)
x = 0
for imgurl in blog_imglist:
    urllib.urlretrieve(imgurl,r'E:\learn-python\blog_spider\%s.jpg' % x)
    x+=1
```

### 结果
得到blog_spider文件和博客中的插图，并将插图按顺序重命名。爬取到的内容如下,一个html文件和五幅图片。

![](http://7rfk7p.com1.z0.glb.clouddn.com/blog_spider.jpg)

### 关于爬虫的反思
这几乎是最简单的爬虫，我尝试利用这种最简单的机制对新浪微博等进行爬取，均告失败，说明还是有很多的内容等着我去学习的。要实现更为复杂的爬虫，难点之一在于正则表达式，一般拿到正确的正则表达式就可以进行无障碍爬取，正则表达式是网络编程的重点和难点，值得花大功夫学习。难点之二在于一些常用高效的爬虫模块的学习，如urllib，urllib2，scrapy等，需要花时间看。此外，许多网站有反爬虫机制，破解也需要具体问题具体分析，多动脑筋。

学习爬虫是一件很有趣的事情，花时间学习正则，模块等基本知识，多思考爬取策略，多看别人的爬虫范例，勤快一点就会有收获的。

