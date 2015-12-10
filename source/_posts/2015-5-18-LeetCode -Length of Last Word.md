title:  Length of Last Word!
date:   2015-05-18 11:23:32
categories: algothrim
tags: algothrim
---

Given a string s consists of upper/lower-case alphabets and empty space characters ' ', return the length of last word in the string.

If the last word does not exist, return 0.

Note: A word is defined as a character sequence consists of non-space characters only.

For example,
Given s = "Hello World",
return 5. 
 
----------

### code

```c++

class Solution {
  public:
    int lengthOfLastWord(string s) {
        if (s.length() == 0) {
            return 0;
        }

        if (s.find(' ') == -1) {
            return s.length();
        }

        int iBeginPos = 0;
        int lastLen = 0;
        int iLastPos = 0;
        char lastChar = ' ';

        bool bFound = false;
        int i = 0;
        for (; i < s.length(); i++) {
            if (' ' == s[i] && ' ' != lastChar) {  // end
                iLastPos = i;
            }
            if (' ' != s[i] && ' ' == lastChar) {  // begin
                iBeginPos = i;
            }
            lastChar = s[i];
        }

        if (iBeginPos == 0 && iLastPos == 0) {
            return 0;
        } else if (s[i - 1] == ' ') {
            lastLen = iLastPos - iBeginPos;
        } else {
            lastLen = i - iBeginPos;
        }

        return lastLen;
    }
};

```