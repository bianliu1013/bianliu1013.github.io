---
layout: post
title: [leetcode] - Reverse Words in a String!
---

Given an input string, reverse the string word by word.

For example,

Given s = "the sky is blue",

return "blue is sky the".

Update (2015-02-12):

For C programmers: Try to solve it in-place in O(1) space. 

----------

### code

```c++
class Solution {
public:
    void reverseWords(string &s) {
        string s_temp;
        int count = s.length() - 1;
        int bFound = -1;
        while(count >= 0)
        {
            // trim
            if(' ' == s[count] && -1 == bFound)
            {
                count--;
            }
            else if(' ' == s[count])  // found space
            {
                if(s_temp.length() != 0)
                   s_temp += ' ';  // add space 
                s_temp += s.substr(count+1, bFound - count);
                bFound = -1; // roll back
            }
            else  // found character
            {
                if(0 == count && -1 == bFound) // only one character
                {
                    if(s_temp.length() != 0)
                        s_temp += ' ';  // add space 
                    s_temp += std::string(1, s[0]);
                }
                else if(0 == count && -1 != bFound)  // 0 position and have other words
                {
                    if(s_temp.length() != 0)
                        s_temp += ' ';  // add space                    
                   s_temp += s.substr(0, bFound + 1); 
                }
                else if(-1 == bFound)  // found first non-words
                {
                    bFound = count;
                }
                count--;
            }
        }
        
        s = s_temp;
    }
};
```