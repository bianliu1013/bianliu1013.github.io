---
layout: post
title: leetcode -  Ugly number.
date:   2015-12-9 11:23:32
categories: leetcode c++
---


 Write a program to check whether a given number is an ugly number.

Ugly numbers are positive numbers whose prime factors only include 2, 3, 5. For example, 6, 8 are ugly while 14 is not ugly since it includes another prime factor 7.

Note that 1 is typically treated as an ugly number. 


### code:
<pre><code>

class Solution {
  public:
    bool isUgly(int number) {
        if(number <= 0)
            return false;
        
        while(number % 2 == 0)
            number /= 2;
    
        while(number % 3 == 0)
            number /= 3;
    
        while(number % 5 == 0)
            number /= 5;
    
        return (number == 1) ? true : false;
    }
};

</code></pre>