---
layout: single
title: "Batch Size Benchmarking"
author_profile: true
date: 2019-03-11 4:30:00
---

# Hello

## Introduction
I learned Convolutional Neural Networks from the point of view of a
technician. As an undergrad, I joined a research lab that had begun
using CNN's for their work. I was tasked with converting a
classification task into a regression. Most of the model architecture
was fixed and already built in Caffe. I was to play with the output
layer and the data pipeline to get continuous values as labels. The
results of that are iinteresting, but left for another post.

As a result of this, I learned to use CNN's through a series of
rules-of-thumb. As I play more with architecture and hyperparameters, I
come into conflict with these rules and, in a series of posts, I'd like
to see where each one comes from.

## The Rule
The first rule-of-thumb I looked at was *It's better if your batch size
is always a power of two*.

## Experiments
To test this, I used a CNN architecture I have been using a lot
recently. The architecture uses a single convolution, followed by four
sets of convolution, batch normalization, hyperbolic tangent activation,
and max pooling, and finally three fully connected layers.  The
specifics and the TensorFlow code for this architecture can be found at
the bottom of this post.

For each batch size I considered, I used full-color image patches with
dimensions 256x256x3, and performed 1000 training iterations. The
training speed in iterations-per-second are shown below.

![]()

But this isn't the full story. What happend when we multiply this by the
number of patches in each iteration to get a total training throughput?
What we find is a graph that looks totally different.

![]()

So what's up? Why is 34 slower than 32, but 64 is just as fast, or
faster?  [This StackExchange
Post](https://datascience.stackexchange.com/questions/20179/what-is-the-advantage-of-keeping-batch-size-a-power-of-2)
Is a great explaination. To sum it up though, my GPU can perform 16
processes at the same time. These processes all take roughly the same
time, so the marginal cost of the 17th or 33rd etc. patch is a whole new
round of computations. The 18th through 32nd patches have very little
overhead though, because a 2nd round of computation has to happen
anyway.

As batch sizes get very large however, the cost of an additional round
of computations is dwarfed by the cost of the existing computations.
This can be seen by looking at the throughput at 18, 34, and 50 patches.


## Results
So is it worth it?

Well it's not just about being a power of two in training. The real key
is to know what kind of parallelism is available to you and how to take
advantage of it. I've found that all recent Nvidia GPU's obey this
multiple-of-16 property. But other devices may act differently.
When considering performance, check your hardware and be sure you're
making the most of your GPU, TPU, or CPU.




If you'd like to characterize your own hardware, or you have questions
about my results, email me at brian@drexel.edu.



