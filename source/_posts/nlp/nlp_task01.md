---
layout:     post
title:      "NLP基础任务及评价指标(一)"
subtitle:   "NLP introduction"
date:       2020-07-17
catalog:    true
author:     "Nova"
header-img: "/img/post-img/0003.jpg"
tags:
    - NLP
---


本文主要包含了NLP的基础任务, 及其评价指标


- NLP的各项技术
- 各项技术的评测指标、数据集
- 各项技术在对应的评测数据集上达到SOTA效果的Paper

了解评测数据集、评测指标和目前最好的结果是NLP研究工作的基础. 



# 按粒度划分任务

词粒度、短语粒度、句子粒度、篇章粒度。 以及对应的一些主要任务。（明确这些NLP基础任务之间的关系）


![](/img/blog/nlp_task1.png)




# 基础任务、核心任务 划分

NLP的基础技术: 围绕不同层次的自然语言处理，比如说分词、词性标注、语义分析做一些加工。后面做任何其他新的技术或者应用都必须要用到基础技术。


NLP核心技术: 包括词汇、短语、句子、篇章的表示，大家所说的Word Embedding就是在研究不同的语言单位的表示方法。它也包括机器翻译、提问和回答、信息检索、信息抽取、聊天和对话、知识工程、语言生成、推荐系统。


NLP+: 仿照“人工智能+”或“互联网+”的概念，实际上就是把自然语言处理技术深入到各个应用系统和垂直领域中。比较有名的是搜索引擎、智能客服、商业智能和语音助手，还有更多在垂直领域——法律、医疗、教育等各个方面的应用。


支撑技术: 用户画像, 包括用来做训练之用的大数据, 云计算提供、实施、训练的基础设施, 包括机器学习和深度学习提供训练的技能, 一定要有各种知识支撑，比如领域知识还有常识知识.




# 百度NLP划分
NLP的基础、技术、应用之间的关系。

## 知识图谱
KnowledgeGraph (Entity Graph,Attention Graph,Intent Graph)

## Application
Search, Feed, O2O, Ads ...

## Big Data

## Machine Learning

## Linguistics


## Language Understanding

## Language Generation


# SOTA
SOTA，全称「state-of-the-art」，用于描述机器学习中取得某个任务上当前最优效果的模型。例如在图像分类任务上，某个模型在常用的数据集（如 ImageNet）上取得了当前最优的性能表现，我们就可以说这个模型达到了 SOTA。



[常见32项NLP任务及其评价指标和对应达到SOTA的paper](https://cloud.tencent.com/developer/article/1422613)

[微软周明：自然语言处理的历史与未来](http://zhigu.news.cn/2017-06/08/c_129628590.htm)

[15大领域、127个任务，这里有最全的机器学习SOTA模型](https://zhuanlan.zhihu.com/p/84419694)


[SOTA模型](https://www.jiqizhixin.com/sota)
