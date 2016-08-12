---
layout: post
title: "Easy Explanation to Temporal Attention"
comments: true
description: "This blog intends to explain the intuition of temporal attention mechanism and how to implement it."
keywords: "temporal, attention mechanism, implement, torch"
---

> This blog intends to explain the intuition of temporal attention mechanism and how to implement it. I would really thank *Ryan Szeto* and *Daiqi Gao* for their careful review of this post.

Attention mechanism is a popular topic in deep learning these years. It is loosely based on human visual attention mechanism, as we tend to focus on a certain region with “high resolution” while perceiving the surrounding image in “low resolution”, and then adjust the focal point over time. Attention is successfully used in [machine translation](https://arxiv.org/pdf/1409.0473v7.pdf), [image captioning](http://arxiv.org/pdf/1502.03044v3.pdf), and [video captioning](http://arxiv.org/pdf/1502.08029v5.pdf) tasks (these are the very first work using attention in their tasks). As I'm working on image/video captioning these days, I'll explain how to apply attention to this specific topic. To begin with, let's go through the approach of image caption generation.

# Image captioning task
The deep learning approach on this task has shown great progresses beyond traditional template approach (like [Integrating Language and Vision to Generate Natural Language Descriptions of Videos in the Wild](http://www.cs.utexas.edu/~ml/papers/thomason.coling14.pdf), a pretty recent work). In most circumstances, it use a CNN to encode an image to a vector as the representation, and use machine translation pipe line to *translate* image feature into target language. That's the typical [encoder-decoder framework](https://arxiv.org/pdf/1406.1078v3.pdf).

# From image to video
When it goes to video captioning, you'll get a set of feature vectors over all frames. In this case, we still want to use image caption model, but that model only support one feature input. Here an idea comes up that we can combine a set of features to a single feature, then simply use image caption model. Clever! So how can we achieve that? The easiest way is to calculate the mean value of all vectors, thus get a single vector representation. That's exactly the work of [Translating Videos to Natural Language Using Deep Recurrent Neural Networks](https://arxiv.org/pdf/1412.4729v3.pdf). Here is a picture of their model.

![2016_08_11_93a0c6c07a264f9f89e1fd1bc376861](http://oa5omjl18.bkt.clouddn.com/2016_08_11_93a0c6c07a264f9f89e1fd1bc376861.png "Structure of video captioning system using mean pooling")

Obviously, this approach fails to consider the temporal order of frames, and gives the same input to the decoder in every time step (as shown in the figure above). We can easily improve this model by replacing the arithmatic mean with weighted average. That's exactly the method in [Describing Videos by Exploiting Temporal Structure](http://arxiv.org/pdf/1502.08029v5.pdf). Here is the picture of their model.

![2016_08_11_76334843cd90cd7af03e2483df49e78](http://oa5omjl18.bkt.clouddn.com/2016_08_11_76334843cd90cd7af03e2483df49e78.png "Structure of video captioning system using temporal attention")

# Temporal attention
So how do we choose those weights? We can calculate the weights automatically! Let's check the structure above. When you want to calculate the weight for a single CNN output, what should be the input factors? We know that the feature itself holds the information about that frame, and the hidden states holds the information of a RNN. As LSTM has a memory over time steps, the information will be gathered into the latest hidden state (_i.e._ the hidden state of previous time step). Thus, it comes naturally to define the weight as a function of a feature and previous hidden state.

$$e_{i}^{(t)} = f(v_i, h^{(t-1)})$$

In this equation, $$v_i$$ represents a feature vector, and $$h_{t-1}$$ is the previous hidden state of which unit the weighted feature is going to input to. And assume $$N$$ is the number of frames and $$T$$ is the total number of time steps in RNN, we will have $$i\in [1,N]$$ and $$t\in [1,T]$$. (We can define $$h^{(0)}$$ as $$\mathbf{0}$$ vector so $$t=1$$ makes sense)

But this doesn't make sure $$e_{i}^{(t)}$$ will have a sum to 1 over all features (_i.e._ $$\sum_{i=1}^Ne_i^{(t)}\neq 1$$). So we apply softmax over them as follows.

$$\alpha_{i}^{(t)} = \frac{exp\{e_{i}^{(t)}\}}{\sum_{i=1}^Nexp\{e_{i}^{(t)}\}}$$

This will give you $$\sum_{i=1}^N\alpha_{i}^{(t)}=1$$, so we can calculate a single feature for each time step as

$$\phi^{(t)}=\sum_{i=1}^N\alpha_{i}^{(t)}v_i$$

## Some details
So how we define $$f(v_i,h^{(t)})$$? That's much the similar of what we do in a node of neural network. Just a linear function over $$v_i$$ and $$h^{(t)}$$ and add a non-linear active function. Here we use $$\tanh$$ as the active function.

$$e_{i}^{(t)} = w^T\tanh(W_ah^{(t-1)}+U_av_i+b_a)$$

In which $$w,W_a,U_a,b_a$$ are all learnable parameters. Here we assume $$r$$ to be the size of an RNN hidden state, $$m$$ to be the encoding size of CNN, $$a$$ to be the attention matrix size (you can play with this parameter), we can list the size of those variables as follows (I always find it useful to list the size of those variables!)

$$
v_i\in \mathbb{R}^{m} \\
h^{(t)}\in \mathbb{R}^{r}\\
W_a\in \mathbb{R}^{a\times r}\\
U_a\in \mathbb{R}^{a\times m}\\
b_a, w \in \mathbb{R}^{a}
$$

So this gives $$e_i^{(t)}$$ as a scalar value.

Another detail is that we share those learnable parameters for different time steps, just like what RNN does. It can be implemented into the RNN unit so that the unit like LSTM can take a sequence as input. And as the equations are all differential, we can learn those parameters using back propagation with any optimization method!

This mechanism can definitely be used in other problems as long as you can transform the problem to a sequence representation. Theoretically, it will perform better than input a single item of a sequence because that can be described as a one-hot weight (_i.e._ weight over the sequence with a vector that only a single position takes 1 and others are all 0). However, if the attention matrix size $$a$$ is too small, intuitively, it won't cover all the possible weights to minimize the cost function, so the choice of $$a$$ is important. But I haven't found a best practice on how to choose that. Currently I just set it the same as $$r$$ (the size of RNN hidden state). Tell me if you have some experience!

# Torch implementation of attention
[Here](https://gist.github.com/chaonan99/766341e72c63763e028eab9428587f24) is a easy implementation of this temporal attention mechanism in [torch](https://github.com/torch). Also shown as follows:

{% highlight Lua %}
require 'nn'
require 'nngraph'

-- torch.manualSeed(123)

seq_per_video = 5
att_size = 512
rnn_size = 512
input_size = 512
frame_per_video = 5

feats = nn.Identity()()
prev_h = nn.Identity()()

local e = {}
local featTable = {nn.SplitTable(1)(feats):split(frame_per_video)}
local nh = nn.Linear(rnn_size,att_size)(prev_h) -- Calculate this outside the loop to save time
local ua = nn.Linear(input_size,att_size) -- Share parameters
local va = nn.Linear(att_size,1)

for i = 1, #featTable do
  local ua_t = ua:clone('weight', 'bias', 'gradWeight', 'gradBias')
  local va_t = va:clone('weight', 'bias', 'gradWeight', 'gradBias')
  e[i] = va_t(nn.Tanh()(nn.CAddTable()({nh,nn.Replicate(seq_per_video)(ua_t(featTable[i]))})))
end

-- When in this output, you should see the attention for each frame *roughly the same* because of random initialization
jo = nn.SoftMax()(nn.JoinTable(2)(e))
m = nn.gModule({feats,prev_h},{jo})

-- Whole attention model
-- ca = nn.MM()({jo,feats})
-- m = nn.gModule({feats,prev_h},{ca})

local in_feat = torch.rand(frame_per_video, input_size)
local in_prev_h = torch.rand(seq_per_video, rnn_size)
print(m:forward({in_feat, in_prev_h}))

{% endhighlight %}
Notice that here is also a `seq_per_video` parameter, in which case there are multiple ground truth sentences for one video clip. This cause the hidden state to be the size of `seq_per_video * rnn_size`, so we need to replicate $$U_av_i$$ to match the size of $$W_ah^{(t-1)}$$.

I'm doing experiment on some models using attention mechanism. I may update my blog to make some comparison with different models in the coming post (as long as I cure my procrastination :-).