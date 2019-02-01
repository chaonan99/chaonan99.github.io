---
layout: post
title: "Python Inverse of Zip"
comments: true
date: "2019-01-13"
description: ""
keywords: "python, zip"
---

An interesting technique I learned today, `unzip` the list of tuple into multiple lists, from [stack overflow](https://stackoverflow.com/questions/12974474/how-to-unzip-a-list-of-tuples-into-individual-lists).

```python
def multiple_list(a):
    return a, str(a), a+1

unpack = lambda x: list(map(list, zip(*list(x))))
test_list = [1, 2, 3]
l1, l2, l3 = unpack(map(two_list, test_list))
```

`l1`, `l2`, `l3` are `[1,2,3]`, `['1','2','3']` and `[2,3,4]` respectively.

