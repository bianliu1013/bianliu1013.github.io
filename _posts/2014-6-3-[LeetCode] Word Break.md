---
layout: post
title: [leetcode] - Word Break!
---

 Given a string s and a dictionary of words dict, determine if s can be segmented into a space-separated sequence of one or more dictionary words.


For example, given

s = <font color=red>"leetcode"</font>,

dict = <font color=red>["leet", "code"]</font>.


Return true because "leetcode" can be segmented as "leet code". 


----------




### code:

``` c
class Solution {
  public:
    bool wordBreak(string s, unordered_set<string>& wordDict) {
        if (wordDict.size() == 0 && s.size() != 0)
            return false;

        if (wordDict.size() == 0 && s.size() == 0)
            return true;

        vector<bool> dpArray(s.length(), false);

        for (int i = 0; i < s.length(); i++) {
            for (int j = i + 1; j <= s.length(); j++) {
                if ((i - 1) >= 0 && false == dpArray[i - 1]) {
                    break;
                }
                if (wordDict.find(s.substr(i, j - i)) != wordDict.end()) {
                    dpArray[j - 1] = true;
                }
            }
        }

        return dpArray[s.length() - 1];
    }
};

```