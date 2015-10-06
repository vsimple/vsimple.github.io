---
layout: post
title: "Hello World"
description: ""
category: start
tags: [intro]
---
{% include JB/setup %}

## Overview

#### 我的第一个程序

{% highlight c linenos %}

int main()
{
    printf("Hello, World");
}
{% endhighlight %}

{{ page.content | number_of_words }}


{{ site.time | date_to_string }}

