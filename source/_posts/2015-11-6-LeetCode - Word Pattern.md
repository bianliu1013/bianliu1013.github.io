---
layout: post
title: leetcode -  Word Pattern
date:   2015-11-6 15:23:32
categories: leetcode c++
---





Given a pattern and a string str, find if str follows the same pattern.

Here follow means a full match, such that there is a bijection between a letter in pattern and a non-empty word in str.

Examples:

    pattern = "abba", str = "dog cat cat dog" should return true.
    pattern = "abba", str = "dog cat cat fish" should return false.
    pattern = "aaaa", str = "dog cat cat dog" should return false.
    pattern = "abba", str = "dog dog dog dog" should return false.

Notes:
You may assume pattern contains only lowercase letters, and str contains lowercase letters separated by a single space. 


### code:
<pre><code>
class Solution {
  public:
    bool wordPattern(string pattern, string str) {
        std::vector<string> splict_strings;
        int l = pattern.length();
        int s = 0;
        int l_offset = 0;
        int offset = str.find_first_of(' ', 0);
        while (offset != string::npos) {
            splict_strings.push_back(str.substr(l_offset, offset - l_offset));
            s++;
            l_offset = offset + 1;
            offset = str.find_first_of(' ', l_offset);
        }

        splict_strings.push_back(str.substr(l_offset, str.length() - l_offset));
        if (l != (s + 1)) {
            return false;
        }

        std::map<char, string> str_map;
        std::map<string, char> str_map2;
        for (int i = 0; i < pattern.length(); i++) {
            if (str_map.end() == str_map.find(pattern.at(i))) {
                str_map[pattern.at(i)] = splict_strings[i];
                str_map2[splict_strings[i]] = pattern.at(i);
                if (str_map.size() != str_map2.size())
                    return false;
            } else {
                auto a = str_map.find(pattern.at(i));
                if (0 != a->second.compare(splict_strings[i])) {
                    return false;
                }
            }
        }

        return true;
    }
};
</code></pre>