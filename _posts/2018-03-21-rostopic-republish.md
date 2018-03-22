---
layout: post
title: "Republish ROS Topic with Different Rate"
comments: true
date: "2018-03-21"
description: "A snippet used to republish ROS topic in a different rate, written in Python."
keywords: "ros, rostopic, publisher, subscriber"
---

Recently I'm using [YOLO in ROS](https://github.com/leggedrobotics/darknet_ros). The full model trained on MS COCO runs super slow (30 s per frame) on my tiny old CPU. But as buying a good GPU or delve into the ROS binding code costs extra efforts, I choose to do a little cheat by repeating the same result message when waiting for new detection result. Surprisingly, I failed to find a tool that can republish a ROS topic with higher rate, although it does have a [throttle tool](http://wiki.ros.org/topic_tools/throttle). So I end up writing a simple [Python snippet](https://gist.github.com/chaonan99/57b28071e36ff381825779de10af5792) that can republish a given topic with a different rate.

<code data-gist-id="57b28071e36ff381825779de10af5792"></code>

## Code Explained
I spend sometime figuring out how subscriber in rospy works. Firstly, the subscriber fork a new thread and run the callback function (`msg_callback` in our case) when new message comes, but only when the last callback returns (otherwise it just keep running the previous one). That is to say, if we put the publisher in the callback, it will fall into an infinite loop and keep publishing the same message.

Thus, we declare a global variable `g_msg`.
<code data-gist-id="57b28071e36ff381825779de10af5792" data-gist-hide-footer="true" data-gist-line="7-8"></code>
And run the publisher loop in a single thread.
<code data-gist-id="57b28071e36ff381825779de10af5792" data-gist-hide-footer="true" data-gist-line="42-43"></code>
<code data-gist-id="57b28071e36ff381825779de10af5792" data-gist-hide-footer="true" data-gist-line="21-30"></code>
The subscriber callback only update this global variable. The publisher is accessing the same variable updated by `msg_callback`.

## Usage
Here is a simple increasing number publisher:
<code data-gist-id="717f96d58aaad169388332cfafe60725"></code>

Starting the publisher, and run the message republish tool by,

    $ python ros_message_republish.py /addup_number 0.5
    $ rostopic echo /addup_number_resampled

This will show

    data: 8
    ---
    data: 10
    ---
    data: 12
    ---
    data: 14
    ---
    data: 16

On the other hand, if the desired rate is higher than publishing rate of the original topic,

    $ python ros_message_republish.py /addup_number 2
    $ rostopic echo /addup_number_resampled


This gives

    data: 23
    ---
    data: 24
    ---
    data: 24
    ---
    data: 25
    ---
    data: 25
    ---
    data: 26
    ---
    data: 26


