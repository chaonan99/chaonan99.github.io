---
layout: post
title: "我的第一篇"
comments: true
description: "My first blog"
keywords: "markdown, typography blog, dummy content"
---

> This is my first blog, which introduce how this personal page is set up and my thought of Markdown.

# 这个网站是如何搭建的
虽然不敢以程序员自居，但是我一直很欣赏程序员的分享精神，学会了什么技术总想写个tutorial，让后来人少走弯路，顺便装装逼也是极爽的。然而使用现成的博客框架，能自己定义的部分太少，页面上元素也太多。另外一种选择就是自己建立一个主页，看到很多人都这么干，于是在今天早上也自己搞了一个。这个主页是基于[Github Page](https://pages.github.com/)的，并使用[Jekyll](https://jekyllrb.com/)生成页面。主要参考了[这篇](http://www.jianshu.com/p/8f843034c7ec)教程。
其实想要搭建自己的主页非常简单，并不一定要程序员才能学会。你甚至不需要像教程里那样自己创建一个，直接在网上找一个jekyll的模板，fork到自己的github中，改掉项目名称就可以用了。你也不需要购买域名，那只是为了给你的主页起个比较正式的名字。如果你不介意使用`yourusername.github.io/`这样的域名，完全可以免费拥有一个个人主页。

# Markdown写作
我第一次接触Markdown貌似是今年年初的时候。之前写文章排版都用LaTex，排版出来效果确实非常棒，然而可能是我用的不够熟练，或者没找到一个好的编辑工具（我用的TexWorks），一直没法做到“所想即所得”，仍然会把很多时间用于排版，文档一长，想要修改中间的章节，定位也很费事。之前就有同学告诉我，他已经开始用Markdown替代LaTex做一些工作。我第一次体验Markdown就被深深吸引了，it's just what I need！不过用了这么久，Markdown还是有些问题令我非常头疼。

* Markdown没有统一的解释器，往往自己电脑上看着正确的文本放到git上就不对了。
* Markdown贴图片稍微麻烦点，但[这里](https://github.com/chaonan99/markdown-image-copy-windows)提供了一个解决方案（顺便给自己打广告hhh）。
* 很多网站的Markdown不支持数学公式。然而既然有了自己的主页，[这个](http://stackoverflow.com/questions/10987992/using-mathjax-with-jekyll)就可以自己添加啦，非常简单。

<div>
$$\sin^2(x) + \cos^2(x) = 1$$
</div>

* Markdown难以转化为其他格式，尤其是PDF。这对于有写作业需求的我还是比较重要的。目前我用的解决方案是用[Rmarkdown](http://rmarkdown.rstudio.com/authoring_quick_tour.html)提供的工具。当然这个机制肯定是不依赖于R语言的，只是我懒得把它拆解出来能用就好 = =

# Future
这个博客的中文字体还没设置（还不会耶），以及可能添加一些搜索，统计的功能。之后继续完善吧！以及开这个主页是为了给自己英文写作的锻炼机会，不知道咋回事就变成用中文了。