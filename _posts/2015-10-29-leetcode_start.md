---
layout: post
title: "LeetCode_start"
description: ""
category: LeetCode
tags: [LeetCode, start]
---
{% include JB/setup %}

##LeetCode
今年暑假，我在LeetCode上做过部分简单题。今天再次打开，发现网站增加了一些功能，
比如可以用自己构造的测例测试程序；

####目录
---
* [1 线性表](#1) 
	*  [1.1 Remove Duplicates from Sorted Array](#1.1) 

---
	
<h4 id="1">1 线性表</h4>
<h4 id="1.1">1.1 Remove Duplicates from Sorted Array</h4>

####问题描述：
> Given a sorted array, remove the duplicates in place such that each element appear only once and return the new length.
> Do not allocate extra space for another array, you must do this in place with constant memory.
> For example,
> Given input array nums = [1,1,2],
> Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively. It doesn't matter what you leave beyond the new length.
> Subscribe to see which companies asked this question

####问题解决：

{% highlight c++ linenos=table %}
class Solution {                                                       
public:
    int removeDuplicates(vector<int>& nums) {
        if( nums.empty() )
            return 0;
			
        int index = 0;
        int n = nums.size();
        for( int i = 1; i < n; i++ )
            if( nums[ index ] != nums[ i ] )
                nums[ ++index ] = nums[ i ];
				
        return index + 1;
    }
};                                                                 
{% endhighlight %}

####问题分析：
类似于插入排序，数据分为两类——已经插入的数据和等待插入的数据。而根据数据最大的重复数，设置插入条件即可。
比如上例的重复数最大为1，即每个数都不同。如果最大可以重复2，则条件可以改为nums[index-1] 和 nums[i]比较。
依次类推，如果最大重复度m, 则条件改为nums[index-m+1] 和 nums[i]比较。当然，变量初始值也要作相应变化。

####问题扩展：

{% highlight c++ linenos=table %}
class Solution {                                                       
public:
    int removeDuplicates(vector<int>& nums) {
        int n = nums.size();
        if( n <= 2 )
            return n;
			
        int index = 1;
        for( int i = 2; i < n; i++ )
            if( nums[ index - 1 ] != nums[ i ] )
                nums[ ++index ] = nums[ i ];
				
        return index + 1;
    }
};
{% endhighlight %}
