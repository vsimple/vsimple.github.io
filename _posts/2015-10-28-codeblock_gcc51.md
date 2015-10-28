---
layout: post
title: "CodeBlock_gcc5.1"
description: ""
category: CodeBlocks
tags: [CodeBlocks]
---
{% include JB/setup %}

##升级CodeBlocks编译器版本
问题：学习C++ Primer，在正则表达式部分，需要用到头文件regex，但由于gcc版本太低，需要升级4.9以上才支持新特性。

####1.下载CodeBlock，版本为codeblocks-13.12mingw-setup-TDM-GCC-481.exe
![有帮助的截图]({{ site.url }}/assets/images/CB_gcc/downCB.gif)

####2.下载TDM-GCC，网址为http://tdm-gcc.tdragon.net/，选择默认安装即可

####3.设置CB的编译器，步骤：setting->Compiler...->Toolchain executables
	（1）先修改编译器安装路径
	（2）再将Program Files框中的文件改成对应的文件，即使名字相同也要进行选择相应文件的操作。
	
![有帮助的截图]({{ site.url }}/assets/images/CB_gcc/setCB.gif)

####4.测试成功。
