---
layout: post
title:  "TF-IDF提取文章关键词算法"
date:   2018-10-27 17:45:38
categories: 数据挖掘
tags:  文本挖掘 数据挖掘 中文分词
---

* content
{:toc}
## 一、TF-IDF简介

TF-IDF（terms frequency-inverse document frequency）是一种用于信息检索与文本挖掘的常用加权技术。TF-IDF是一种统计方法，用来评估一字词对于一篇文章的重要程度。一个词语对一篇文章的重要性主要是依靠它在文件中出现的次数，如果这个词语在这篇文章中的出现次数越高，则表明这个词语对于这篇文章的重要性越高。同时，它还与这个词语在语料库中出现的文章篇数有关，随着出现的篇数越多，则会降低这个词语在这篇文章中的重要性，具体的算法请看下面。

## 二、算法实现

1、在实现这个算法之前，我们需要对一篇文章进行分词，在进行中文分词的时候，推荐一个python库，jieba分词，作者将这个项目发布到了GitHub上，是开源的，GitHub地址https://github.com/fxsjy/jieba

2、TF词频的计算

词频（TF）=某个词语在文章中的出现次数

由于我们需要考虑不同的文章，长度不同，我们需要将词频进行归一化处理

词频（TF）=某个词语在文章中的出现次数/文章的总词数 或者 词频（TF）=某个词语在文章中的出现次数/这篇文章出现最多的词的出现次数

3、IDF的计算

逆文档频率（IDF）=log（语料库的文档总数/包含该词的文档数+1），语料库可以自己去网上下载，计算逆文档频率的原因是为了去除哪些经常出现的词语，比如说，“的”、“我们”、“他”等这类的词语，这些词语对于整篇文档重要性不高、但是出现的频率会比较多，就有可能会影响到我们最后的计算结果，如果是经常出现的词语就不能作为我们文章的关键词。

4、计算TF-IDF的值

TF-IDF = 词频（TF）* 逆文档频率（IDF）

5、排序

对文章词语的TF-IDF值进行排序，我们可以选择提取TF-IDF值比较大的词语

6、总结

TF-IDF算法的优点是简单快速，结果比较符合实际情况。但，TF-IDF算法是单纯的以“词频”来衡量一个词的重要性，就显得不够全面，这些词语就不一定能体现出文章的主要思想突出文章的主题。而且，这种算法也无法体现出词语所处的不同位置对于文章的重要性不同，如果想解决这个问题，我们可以采用对于词语所处的不同位置给他们设定不同的权重。

## 三、测试案例

下面的例子是使用jieba库，来实现TF-IDF算法的，下面是文章的内容


有很多不同的数学公式可以用来计算tf-idf。
这边的例子以上述的数学公式来计算。
词频（tf）是一词语出现的次数除以该文件的总词语数。
假如一篇文件的总词语数是100个，而词语“母牛”出现了3次，
那么“母牛”一词在该文件中的词频就是3/100=0.03。
一个计算文件频率（DF）的方法是测定有多少份文件出现过“母牛”一词，
然后除以文件集里包含的文件总数。所以，如果“母牛”一词在1,000份文件出现过，
而文件总数是10,000,000份的话，其逆向文件频率就是log（10,000,000 / 1,000）=4。
最后的tf-idf的分数为0.03 * 4=0.12。
## python代码实现
```
import sys
sys.path.append('../')
 
import jieba
import jieba.analyse
from optparse import OptionParser
 
file_name = "../txt/test.txt"
 
content = open(file_name, 'rb').read()
 
#10表示输出的前10个
tags = jieba.analyse.extract_tags(content, topK=10)
 
print(",".join(tags))

```
输出结果

000,文件,母牛,词语,tf,词频,100,idf,10,0.03