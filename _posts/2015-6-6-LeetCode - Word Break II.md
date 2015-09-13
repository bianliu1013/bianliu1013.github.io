---
layout: post
title: leetcode -  Word Break II!
date:   2015-06-06 11:23:32
categories: leetcode c++
---


 Given a string s and a dictionary of words dict, add spaces in s to construct a sentence where each word is a valid dictionary word.


Return all such possible sentences.


For example, given

s = <font color=red>"catsanddog"</font>,

dict = <font color=red>["cat", "cats", "and", "sand", "dog"]</font>.


A solution is <font color=red>["cats and dog", "cat sand dog"]</font>. 



### code:

``` c

class Solution {
    unordered_map<string, vector<string>> dp_map;
  public:
    vector<string> wordBreak(string s, unordered_set<string>& wordDict) {
        vector<string> final_str;
        if (wordDict.size() == 0) {
            return final_str;
        }

        auto it = dp_map.find(s);
        if (it != dp_map.end()) {
            return it->second;
        }

        for (int i = 0; s.length() > 0 && i < s.length(); i++) {
            string cur_str = s.substr(0, i + 1);
            string left_str = s.substr(i + 1, s.length() - i - 1);

            if (wordDict.end() != wordDict.find(cur_str)) {
                vector<string> str = wordBreak(left_str, wordDict);
                if (str.size() > 0) {
                    for (auto it: str) {
                        string ss = cur_str;
                        if (it.length() > 0) {
                            ss =  ss + " " + it;
                        }
                        final_str.push_back(ss);
                    }
                } else if(left_str.length() == 0) {
                    final_str.push_back(cur_str);
                }
            }
        }

        dp_map.insert(std::make_pair(s, final_str));
        return final_str;
    }
};


```