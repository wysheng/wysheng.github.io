---
layout: post
title:  "使用python提取文章关键词"
date:   2018-9-25 17:45:38
categories: 数据挖掘
tags:  文本挖掘 数据挖掘 python
---

* content
{:toc}
提取文章关键词，使用TF-IDF 算法，使用的例子是结合jieba分词，使用FreDist，因为TF-IDF算法需要的是一个语料库，当前语料库只有一篇文章，所以TF-IDF算法就退化成计算文章词频的算法了：
需要记录的是FreqDist的成员函数
	plot(n)，绘制出现次数最多的前n项
	tabulate(n)，该方法接受一个数字n作为参数，会以表格的方式打印出现次数最多的前n项
	most_common(n)，该方法接受一个数字n作为参数，返回出现次数最多的前n项列表
	hapaxes()，返回一个低频项列表
	max()，该方法会返回出现次数最多的项。
	
代码如下：
```
# -*- coding: utf-8 -*-
import requests
from bs4 import BeautifulSoup
import jieba
import re
from nltk.book import *
from pylab import *
from jieba.analyse import *
def stop_words():
	stop_word_list = []
	f = open('stopwords.txt', 'rU',encoding='UTF-8')
	for word in f:
		stop_word_list.append(word.strip())
	return stop_word_list	
r = requests.get('https://blog.csdn.net/chszs/article/details/806585xx802')
soup = BeautifulSoup(r.text, 'lxml')
# 获得主要内容
context = soup.find('article').get_text()
# 进行结巴中文分词，获得字符串数组
jieba.load_userdict('user_dict.txt')
word_list = jieba.cut(context)
word_list_str = (",".join(word_list))
word_list = re.split(",", word_list_str)
#去掉长度为1的单词，同时去掉停止词
stop_word_list = stop_words()
word_list = [w for w in word_list if (len(w)>1 and (w not in stop_word_list))]
word_freq_list = FreqDist(word_list)
# 根据次品得到前20 项
word_commons = word_freq_list.most_common(20)
for word in word_commons:
	print(word[0], word_freq_list.freq(word[0]))

```
所以下一篇文章中主要解决两个问题，第一个是语料库的问题，我们可以将这个用户的所有文章爬下来，获得该用户的所有文章，然后进行计算，使用TF-IDF算法也就是在当前文章中出现次数最多，在其他文章出现次数越少的越可以代表当前文章，

PS:  

1. 如果发现jieba分词的结果不是很准确的时候，可以通过加载用户自定义词典进行修正 jieba.load_userdict('xxx.txt')，需要注意的是该词典文件应该是utf-8编码的

2. 因为这里文本清理使用的是bs4的Beautiful4soup，因为我爬取的是csdn的博客，该博客的主要内容在article标签当中，所以只取article的get_text()即可
