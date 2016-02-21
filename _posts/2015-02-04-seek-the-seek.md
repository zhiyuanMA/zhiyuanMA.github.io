---
layout: post
title: seek the 'seek'
description: 
category: work
---

闲来无事打算研究一下[seek](http://www.seek.co.nz)上关于JAVA的工作要求。无奈的发现seek的搜索结果页每一个命中项，居然是自动生成的链接，这意味着无法通过抓取获得每一个职位的地址，令人发指！我只好手动取了前60个职位进行分析。


搜索条件 -- keywords: java & classification: any & location: all & date listed: last 30 days & work type: full time


首先循环爬取每个职位的页面描述，然后进行词频统计。删去停用词，以及不相关词后，词云如下：

![词云1](/images/seek-1.png)

所以JAVA CODER一定要有WEB经验，不仅仅是SPRING, 持久层的HIBERNATE，后台DB，甚至前端语言都要熟悉，不是FULL-STACK也要是GENERALIST。另外沟通也是一项极其重要的技能，所以**口语**不好的赶紧练习吧。另外，我还看到HADOOP,NOSQL也名列前茅，我心甚慰。

没有出现在词云图上，但词频最高的是EXPERIENCE，可见在纽西兰最重要的是**经验**啊

那其他要求呢：

![词云2](/images/seek-2.png)


这幅图太明显了，牛人恒牛，技术本领高，还会做管理，哪里都需要，还要充满激情与自信。