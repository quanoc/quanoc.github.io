---
layout:     post
title:      "FastText实现流程"
subtitle:   "What is fastText"
date:       2019-09-18
catalog:    true
author:     "Nova"
header-img: "/img/post-img/0003.jpg"
tags:
    - fasttext
    - NLP
---

Facebook于2016年开源的一个**词向量计算**和**文本分类**工具。
它的优点非常明显，在文本分类任务中,fasttext(浅层网络)往往能取得和深度网络相媲美的精度，却在训练时间上比深度网络快许多数量级。



1. this.readLineTokens(line)【拆分token】 将输入的文本 按照空格分隔成 一组词(tokenList),并以 </s> 结尾
2. getDictLine(tokens,lineWords,labels)【特征提取】
 - addSubwords(lineWords, token, wid);【根据wid获取ngrams特征:直接从字典获取(未登录词,通过token计算)】
 - addWordNGrams(lineWords, wordHashes, WordNgrams:2)【词的 ngram 特征:根据词的hash 滑动计算】
 【h = h * coeff  + h2】【h = h * coeff - h2】
3. lineWords就是计算的特征结果. int[] input = Ints.toArray(lineWords)【转为数组】
4. this.predict(words, predictions, k, threshold)【预测】
 - 定义hidden层为dim维向量；output为label的个数
 - input -> hidden【】
 - hidden -> output【】
 - output -> MinMaxPriorityQueue
 - MinMaxPriorityQueue -> List<FastTextPrediction>【转换为标签】


```java
// 隐层计算: 由输入层求和并平均，乘以权重矩阵A
public void computeHidden(int[] input, Vector hidden) {
    Preconditions.checkArgument(hidden.size() == hsz);
    hidden.zero();
    for(int it : input) {
      if (quant) {
        hidden.addRow(qwi, it);
      } else {
        hidden.addRow(wi, it);
      }
    }
    hidden.mul(1.0f / input.length);
  }
```


```java
// 输出层计算：由隐层乘以权重矩阵B得到的
  public void computeOutputSoftmax(Vector hidden, Vector output) {
    if (quant && args.getQOut()) {
      output.mul(qwo, hidden);
    } else {
      output.mul(wo, hidden);
    }
    float max = output.at(0), z = 0.0f;
    for (int i = 0; i < osz; i++) {
      max = Math.max(output.at(i), max);
    }
    for (int i = 0; i < osz; i++) {
      float p = (float) Math.exp(output.data[i] - max);
      z += p;
      output.set(i, p);
    }
    for (int i = 0; i < osz; i++) {
      output.data[i] /= z;
    }
  }
```


# 总结
- n-gram包含有字粒度的n-gram和词粒度的n-gram

## 1. 核心参数
- dim: 词向量的维度
- minn: subwords的长度范围
- maxn: subwords的长度范围
- loss:
- wordNgrams:
- epoch 和 -lr ：轮数和学习率，default是5和0.05
- thread: 运行的线程数


## 2. 隐层和输出层计算
```
隐层：对于cbow，简单的求和；
对于fastText，隐层是由输入层求和并平均，乘以权重矩阵A得到的。相当于将各个词向量加权求和，作为该句子的vector。
（CBOW会将上下文单词向量叠加起来并经过一次矩阵乘法（线性变化）并应用激活函数）
```

## 3. 关于分类效果
假设我们有两段文本：
我 来到 达观数据
俺 去了 达而观信息科技
```
但在传统的分类器中，用来表征这两段文本的向量可能差距非常大。传统的文本分类中，你需要计算出每个词的权重，比如tfidf值， “我”和“俺” 算出的tfidf值相差可能会比较大，其它词类似，于是，VSM（向量空间模型）中用来表征这两段文本的文本向量差别可能比较大。
```

```
但是fastText就不一样了，它是用单词的embedding叠加获得的文档向量，词向量的重要特点就是向量的距离可以用来衡量单词间的语义相似程度，于是，在fastText模型中，这两段文本的向量应该是非常相似的，于是，它们很大概率会被分到同一个类中。
```

> 使用词embedding而非词本身作为特征，这是fastText效果好的一个原因；另一个原因就是字符级n-gram特征的引入对分类效果会有一些提升 。

