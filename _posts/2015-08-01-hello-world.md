---
layout: post
title: "Hello World"
description: ""
category:
tags:
---
{% include JB/setup %}

## Overview
我y

	int main()
	{
		printf("Hello, World");
	}

### What is Jekyll?

Jekyll is a parsing engine bundled as a ruby gem used to build static websites from
dynamic components such as templates, partials, liquid code, markdown, etc. Jekyll is known as "a simple, blog aware, static site generator".

    .
    |-- _config.yml
    |-- _includes
    |-- _layouts
    |   |-- default.html
    |   |-- post.html
    |-- _posts
    |   |-- 2011-10-25-open-source-is-good.markdown
    |   |-- 2011-04-26-hello-world.markdown
    |-- _site
    |-- index.html
    |-- assets
        |-- css
            |-- style.css
        |-- javascripts
		

- **\_config.yml**
	Stores configuration data.
		
    ---
    title :  Hello World
    categories : [lessons, beginner]
    ---
