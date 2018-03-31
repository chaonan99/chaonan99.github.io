---
layout: post
title: "Source Multiple setup.zsh in ROS w/o Overlaying"
comments: true
date: "2018-03-31"
description: "How to source multiple setup files in ROS"
keywords: "ros, catkin_make"
---

To use packages in your own workspace in ROS, you'll have to source the setup file:

    source ($your_catkin_ws)/devel/setup.*sh

But when you have multiple workspaces, which I do to keep things in order and separate the build process of each set of packages, sourcing one of the setup file will cause [overlaying](http://wiki.ros.org/catkin/Tutorials/workspace_overlaying). There are some discussions on extending the package path rather than overlaying behavior on [GitHub](https://github.com/catkin/catkin_tools/issues/48).

But basically you can make change of `devel/_setup_util.py`, search for `CMAKE_PREFIX_PATH` and change the following line (line 266 for me).

{% highlight Python %}
# environment at generation time
CMAKE_PREFIX_PATH = '($catkin_ws_A)/devel;($catkin_ws_B)/devel;/opt/ros/indigo'.split(';')
# prepend current workspace if not already part of CPP
{% endhighlight %}

You can add other workspace path into a single file, and do not have to source from multiple workspaces. This is apparently not the desired solution but solve the problem for me...