---
layout: post
title: leetcode - Longest Valid Parentheses!
date:   2015-05-17 11:23:32
categories: leetcode c++
---

Given a string containing just the characters '(' and ')', find the length of the longest valid (well-formed) parentheses substring.

For "(()", the longest valid parentheses substring is "()", which has length = 2.

Another example is ")()())", where the longest valid parentheses substring is "()()", which has length = 4. 

----------

### code

```c++

class Solution {
  public:
     int longestValidParentheses(string s) {
        if (s.empty()) {
            return 0;
        }

        string statckString;
        int max_deep = 0;
        int this_deep = 0;
        int pre_deep = 0;
        int i = 0;
        char lastChar = 0;
        int lastTail = 0;
        std::vector<int> dp;
        dp.resize(s.length());


        while (s[i] == ')') {
            i++;
        }

        statckString.push_back(s[i]);
        lastChar = s[i];
        i++;
        while (i < s.length()) {
            if (lastChar == '(' && s[i] == ')') {
                this_deep++;
                statckString.pop_back();

                int last_pos = i - this_deep * 2;
                int last_dp = last_pos == -1 ? 0 : dp[last_pos];
                this_deep += last_dp;
                dp[i] = this_deep;

                if (this_deep > max_deep) {
                    max_deep = this_deep;
                }

                if (statckString.length() > 0) {
                    lastChar = statckString[statckString.length() - 1];
                } else {
                    lastChar = 0;
                }
            } else {
                if (s[i] == '(') {
                    statckString.push_back(s[i]);
                    lastChar = s[i];
                    this_deep = 0;

                } else {
                    this_deep = 0;
                }
            }
            i++;
        }

        if (this_deep > max_deep) {
            max_deep = this_deep;
        }
        return max_deep * 2;
    }

};
```