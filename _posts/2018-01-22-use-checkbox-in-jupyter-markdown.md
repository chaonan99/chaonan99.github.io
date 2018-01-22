---
layout: post
title: "Using Checkbox in Jupyter Markdown"
comments: true
date: "2018-01-22"
description: "A simple solution to create TODO list in jupyter markdown."
keywords: "markdown, jupyter, checkbox"
---

I'm searching for solutions to render checkbox in jupyter notebook, which is a feature provided in GFM.

![](https://cloud.githubusercontent.com/assets/638998/3866320/dea3855e-1fcb-11e4-96ab-7bd11ee246e7.png)

As noted in issue [#6288](https://github.com/ipython/ipython/issues/6288) IPython use [marked.js](https://github.com/chjj/marked) for rendering, but marked.js doesn't have a backend to manage checkbox in an interactive way. One static solution proposed in issue [#107](https://github.com/chjj/marked/issues/107) is to use unicode entities:

* For unchecked square: [http://www.fileformat.info/info/unicode/char/2610/index.htm](http://www.fileformat.info/info/unicode/char/2610/index.htm)
* For checked square: [http://www.fileformat.info/info/unicode/char/2611/index.htm](http://www.fileformat.info/info/unicode/char/2611/index.htm)

Looks like this after rendering:

![](../../assets/images/20180122/jupyter_result.png)

Not very impressive...

Please let me know if there exists better solution!