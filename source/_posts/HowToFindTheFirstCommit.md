---
title: How to find the first commit
date: 2020-08-26 11:00:00
tags: 
- tips
- git
- github
---

## 序
最近在研究一个开源项目，发现它的提交记录有2000+条，而且提交记录的时间跨度非常大，从 2016 年到 2020 年，这让我很困惑，于是想看看这个项目最初是怎么开始的，于是就有了这篇文章。

## 思路
1. 查看项目的提交记录，找到最早的提交记录
2. 查看最早的提交记录的父提交记录，如果父提交记录为空，则说明这是第一个提交记录

## 实现
1. 查看提交记录
```bash
git log
```
2. 查看最早的提交记录的父提交记录
```bash
git show <commit_id>
```
3. 如果父提交记录为空，则说明这是第一个提交记录

## 总结
通过以上步骤，我们可以找到项目的第一个提交记录,但是这个方法需要先将项目克隆到本地，如果项目比较大，可能会比较耗时。碰巧我在github上浏览commit记录时发现了如下信息:(这里以dubbo-kubernetes为例)

当进入commits页面时，会看到如下信息：
![image](/source/_doc/htft1.png)

在本页面只能向前或向后一页跳转，无法直接跳转到第一个提交记录，于是我又尝试了其他方法，发现可以通过如下方法直接跳转到第一个提交记录：

当我点击下一页时,发现地址栏中变为了

`https://github.com/apache/dubbo-kubernetes/commits/master/?after=cbf444c0fc867c55af8903aaced1b350c2f0d732+34`

经过对比发现地址栏中的参数`cbf444c0fc867c55af8903aaced1b350c2f0d732`就是最新的提交记录的SHA，后面的`+34`表示从最新的提交记录开始跳过34条记录，所以我们可以通过如下方法直接跳转到第一个提交记录：

`https://github.com/apache/dubbo-kubernetes/commits/master/?after=cbf444c0fc867c55af8903aaced1b350c2f0d732+<commits总数-2>`

比如该项目目前有2029条commit记录，那么就可以通过如下地址直接跳转到第一个提交记录：

`https://github.com/apache/dubbo-kubernetes/commits/master/?after=cbf444c0fc867c55af8903aaced1b350c2f0d732+2027`

![image](/source/_doc/htft2.png)