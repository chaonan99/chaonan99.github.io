---
layout: post
title: "专业实践思想总结"
comments: true
description: "This blog post is the thinking summary of my summer intern..."
keywords: "GitHub, markdown, latex, camo, sublime, plug-in"
---

这个暑假，我通过自己邮件联系的方式争取到了密歇根大学暑期研修的机会，导师是 [Dr. Jason Corso](http://web.eecs.umich.edu/~jjcorso/)。一次海外研修的经历，确实令我大开眼界，增长了才干，是一次难忘的体验。

## 协作方式
--------------------------------------
国外的学校，研究组十分看重平台的搭建，花费了很多时间考虑如何提高工作效率，让工作更为轻松，可共享性更强。相比之下，我感觉国内的很多研究组在平台这点上还是和国外有很大差距。

### Slack交流
我们组，以及我和[Dr. Deng](http://web.eecs.umich.edu/~jiadeng/)组里的同学交流发现，我们两个组都使用slack协作。[Slack](https://slack.com/)是一个群聊工具，最基本的功能是私信和建立群组。不过它可以支持各种插件，以实现接收邮件，自动提醒等功能，非常适合程序员之间的协作。

### Gitlab仓库
我们组搭建了组内的[gitlab](https://about.gitlab.com/)服务器。这是类似于github的一个代码仓库，只不过只限组内人员访问。我们可以将自己项目的commit提醒嵌入到slack，以便老师和其他组员可以跟进我们的进展。在做计算机视觉方面的项目时，我们经常需使用很多机器同时运行。我就经常使用gitlab来同步不同机器上的代码。

### 计算
我们组研究计算机视觉，对计算资源的需求量巨大。比较简单的是方案使用有很多GPU的服务器，因此我们每次组会的一个重要议题就是要买什么样的GPU。其二是使用Flux集群计算。这个我没有用过，大致是一个将很多计算资源整合使用的东西，我们组之前用过，其他组当然也在用。这个服务是由学校提供，研究组可以租用。另外，一些研究组、公司会使用[AWS](https://aws.amazon.com/)云计算的解决方案。国外有一些课程为学生提供了一些AWS的实例模板，例如 [Stanford CS231](http://cs231n.stanford.edu/)。使用这些实例模板建立的实例，就已经预装了所有需要的软件，例如python，cuda（Nvidia GPU的驱动），[torch](http://torch.ch/)，[caffe](http://caffe.berkeleyvision.org/)等深度学习框架。这样就不需要我们再去花时间安装软件，直接写代码就可以了。我出于好奇，也学习了如何使用AWS的云计算服务。免费版的计算速度还是很慢的，不过可以在其上搭建VPN，博客，论坛等。学会使用这些工具，之后肯定大有用处。

## 导师风格的不同
--------------------------------------
我还在同其他同学的交流之中感受到了不同老师的不同风格。我的导师十分勤奋，在组内起到了很好的示范作用。他自己有自己的项目，和我们大家一样，也会经常写代码，也会写lab notebook并同步到slack上，以便我们check他的进度。与此同时，他还有很多教授需要做的事，例如申请科研基金，到处开会等。他的风格十分亲民。在研修期间，我们全组人都被邀请到他家去开party，其间老师、学生、邻居都有。平时组会的交流也十分平等。我们还要求他也在组会的时候做张ppt展示自己的工作。但相比之下，在Dr. Deng的组里，暑期实习生就不被允许参加大组会，实习生直接和带他们的博士交流，而博士在组会上汇报进展。这样一来，交流也就不那么平等了。不过两种方式也各有好处。我们组虽然平等交流，但由于谁都可以在任何时候打断，开会的效率往往比较低，并不是所有人都能够汇报自己的进度，而没说上的人也没有补偿措施。而有一定等级区分的组，教授就可以控制开会进度，限制大家的发言时间，效率可能会有提高。当然，也有一部分教授不开组会。我和一个戏剧学院的实习生交流时得知，她的导师都是邮件和她单独联系的。总之，不同的导师风格会完全不同，并且会对他的学生产生深刻影响。

## 个人发展
--------------------------------------
个人发展方面，我在科研、编程、生活方面的能力都有很大提高。

### 科研能力
我对我所感兴趣的研究方向有了更深的了解。假期里我阅读了大量的文献，学习了很多教程，对深度学习领域有了更深入的了解，尤其对encoder-decoder、RNN有了深入了解。RNN（Recurrent neural network）是一种人工神经网络，在序列输入的学习任务中占有十分重要的地位。encoder-decoder最初在自然语言处理有了应用，其思想是先用RNN将源语句进行编码，信息储存在RNN单元中，再用另一个RNN输出目标语句。这种方法还被应用于图片、视频描述，只要将encoder的RNN更换为CNN，就可以为图片、视频生成描述了。这也是我暑期研修的研究内容之一。我还广泛了解了其他相关领域，诸如CNN结构的发展，增强学习，对抗生成网络等。

### 编程能力
由于我还参与了项目网页的开发工作，还学习到了网页开发技术，如Flask、Django等，以及网页服务器搭建的知识，学会了搭建Apache服务器，使用Celery队列等技术。暑假期间我经常使用Python，Lua编程。其中Python是现在使用最为广泛的语言之一，在科研中使用尤其多，目前比较流行的深度学习框架，如Tensorflow，Theano也都是基于Python。Lua是一门比较小众的语言，我们的项目使用torch框架，它是基于Lua的。由于Lua没有Python等主流语言那么强大的功能，torch也是由开源社区来维护，文档并不是特别完善，程序debug的难度非常大，这也很大程度增强了我解决问题的能力。平时解决编程问题，往往搜索一下就有现成的解决方案。但调试torch的程序，很多问题的报错都很模糊，只好深入代码去理解原理。虽然总会在错误上花费更多时间，但每个错误都是很好的学习机会。

### 生活能力
我的独立生活的能力也有所提高。而且由于整天一个人独处，对寂寞的忍耐程度也明显上升了。

## 工作方式的进步
--------------------------------------
我的工作方式也发生了很大变化。我对Linux系统的了解更为深入，完全习惯了在Linux上工作，我还更新了实验室的一个服务器系统。其次，我更加体会到开源、共享的重要性。我搭建了自己的博客，坚持在gist上分享自己的代码片并共享到slack，还在github上发起了论文笔记、数据库摘要的项目，实验室的师兄也表示很有兴趣，想要参与其中做贡献。我们的研究就建立在已有的开源代码基础上，我们自然也应该开源自己的代码。这种精神令我深受触动。并且当今的大公司也都抢着开源自己的深度学习框架和工具，希望有更多人使用。可以明显看到，开源是深度学习，乃至其他领域研究的必然趋势。

## 文化氛围
--------------------------------------
研修过程中我感受到，美国和中国的文化氛围有很大不同。

### 对残疾人的重视
他们对残疾人的权利十分重视。学校里几乎所有门都有自动打开按钮，入口也有残疾人通道，基本是无障碍校园。停车场都会设置专用车位。超市里会有专用的电动车。由于这些保障措施，残疾人得以和正常人一样生活、工作。

### 更加开放
美国人比中国人开放得多，我觉得这是由于他们从小就被鼓励发表自己的看法，约束较少。由于美国是移民国家，这里总能见到各个国家的人，和他们交流总能令我大开眼界。美国的消费观念也和中国不同。他们的银行存款利率很低，但信用卡消费总有各种返现、折扣，通过各种措施刺激消费。

### 制度更为健全
美国的制度也相对健全，很少出现诈骗的状况，我所在的城市也很少有暴力犯罪，家里整天不锁门也很安全。美国的一些小路口会有 stop sign，车到达路口必须停车，并且按照到达的先后顺序通过路口。几乎所有路口都会有这个标志，这使得行人在过马路时不必担心过来的车会撞上来，司机开车时也不必担心会忽然钻出一个行人。