---
layout: post
title: "How to Add Equation on Github Markdown File"
comments: true
date:   2016-08-31
description: "This blog post tells you how to show latex equation on GitHub (not GitHub page, but .md file like README)."
keywords: "GitHub, markdown, latex, camo, sublime, plug-in"
---

> This blog post tells you how to show latex equation on GitHub (not GitHub Page, but .md file like README).

Sometimes we may need to write some equation in README file. This will be easy as the markdown file is rendered to HTML (like on GitHub Page). But when we are viewing a README file directly in a repo, those latex style formulas (like $\sigma$) won't work (as is raised [here](http://stackoverflow.com/questions/11256433/how-to-show-math-equations-in-general-githubs-markdownnot-githubs-blog) and [here](https://github.com/STAT545-UBC/Discussion/issues/102)). Luckily, this is achievable through some external services, to request a image of formula through URL. You may just want a [solution](#solution), or know the [reason](#reason) behind it. Also a [Sublime plug-in](https://github.com/chaonan99/Latex2Picture) to do this on pressing a hotkey.

# Solution

1. Write equation in latex style, e.g. `\frac{\sigma}{\mu}`.
2. In [this site](http://www.url-encode-decode.com/) (or either tool for url encoding-decoding), encode your formula. This encode your formula in `%` followed by two hexadecimal digits, e.g. `%5Cfrac%7B%5Csigma%7D%7B%5Cmu%7D` for the example in step one.

![picture of url encoder site](http://oa5omjl18.bkt.clouddn.com/2016_08_31_pasted_image_at_2016_08_31_01_54_pm.png "pasted_image_at_2016_08_31_01_54_pm")

3. Prefixing the encoded formula by `http://latex.codecogs.com/svg.latex?`, e.g. `http://latex.codecogs.com/svg.latex?%5Cfrac%7B%5Csigma%7D%7B%5Cmu%7D`. This url will return a picture of the formula you want. More information [here](http://latex.codecogs.com/eqneditor/integration/htmlequations.php).
4. You can add it as a picture in markdown like `![img](http://latex.codecogs.com/svg.latex?%5Cfrac%7B%5Csigma%7D%7B%5Cmu%7D)`.

# Reason

* Why we need picture rather than direct latex?

GitHub markdown parsing is performed by the [SunDown](https://github.com/vmg/sundown) (ex libUpSkirt) library. For security, it won't allow javascript to be executed when rendering markdown to HTML. Thus won't provide equation feature.

* Why encoding url?

First of all, GitHub uses an open-source project called Camo to provide a proxy for images hosted on GitHub. You will see that once your file is uploaded, the [url of the image will change](https://help.github.com/articles/why-do-my-images-have-strange-urls/) to something like `https://camo.githubusercontent.com/672ecfd312696079a......`. But this mechanism only support encoded url and does not seem to support http redirect (which some service may fail to provide image, like [iText2Img](http://www.sciweavers.org/free-online-latex-equation-editor)).

# Sublime plug-in for this
There is no way for a programmer to do these foolish steps by hand! We always try to solve an easy problem by raising a much more difficult problem... If you know nothing about how to write a sublime plug-in, [here](https://clarknikdelpowell.com/blog/creating-sublime-text-3-plugins-part-1/) is a good start. Yet to implement a practicable plug-in is really difficult.

[Here](https://github.com/chaonan99/Latex2Picture) I wrote an easy sublime plug-in. It can do those steps above for you automatically just after pressing a hot key.

{% highlight Python %}

import sublime, sublime_plugin, urllib

def selections(view, default_to_all=True):
    regions = [r for r in view.sel() if not r.empty()]
    if not regions and default_to_all:
        regions = [sublime.Region(0, view.size())]

    return regions

def quote(view, s):
    return "![equation](http://latex.codecogs.com/svg.latex?" + urllib.parse.quote(s, safe='') + ")"

class LatexPictureCommand(sublime_plugin.TextCommand):
    """
    Main logic
    """
    def run(self, edit):
        view = self.view
        for region in selections(view):
            s = view.substr(region)
            view.replace(edit, region, quote(view, s))

{% endhighlight %}

Feel free to add more function to that easy code!