---
layout: post
title: "Colored cat output"
date: 2013-09-27 23:07
comments: true
categories: [Shell,bash]
---

Today, while checking some old scripts laying around my home directories I found 
myself "cat-ing" a ton of them. **cat** is mighty useful, but It could really use 
a colored output.

I found a small Python program called **Pygmentize**, which is nothing more than 
a syntax highlighter that supports a lot of programming languages and file 
formats.

If you're on Linux and using apt you can find the package by the name of 
*python-pygments*.

Once installed, I just added a simple entry to my *bash\_aliases* file:

``` bash
alias ccat='pygmentize -g'
```

From monochrome to fabulous.

{% img https://s3-us-west-2.amazonaws.com/frncscgmz-blog/ccat.png %}
