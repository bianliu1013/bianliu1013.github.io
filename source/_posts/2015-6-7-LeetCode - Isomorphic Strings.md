title:  Isomorphic Strings!
date:   2015-06-07 11:23:32
categories: algothrim
tags: algothrim
---


Given two strings s and t, determine if they are isomorphic.


Two strings are isomorphic if the characters in s can be replaced to get t.


All occurrences of a character must be replaced with another character while preserving the order of characters. No two

characters may map to the same character but a character may map to itself.


For example,

Given <font color=red>"egg", "add"</font>, return true.


Given <font color=red>"foo", "bar"</font>, return false.


Given <font color=red>"paper", "title"</font>, return true.


Note:
You may assume both s and t have the same length.


### code:

``` c

class Solution {
  public:
    bool isIsomorphic_sub(string s, string t) {
        if (s.length() == 0) {
            return true;
        }

        std::map<char, char> smap;
        for (int i = 0; i < s.length(); ++i) {
            auto it = smap.find(s[i]);
            if (it == smap.end()) {
                smap[s[i]] = t[i];
            } else {
                if (it->second != t[i]) {
                    return false;
                }
            }
        }
        return true;
    }

    bool isIsomorphic(string s, string t) {
        return isIsomorphic_sub(s, t) && isIsomorphic_sub(t, s);
    }
};

```