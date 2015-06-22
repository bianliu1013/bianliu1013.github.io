---
layout: post
title: [leetcode] -  Restore IP Addresses!
---


Given a string containing only digits, restore it by returning all possible valid IP address combinations.


For example:

Given <font color=red>"25525511135"</font>,


return <font color=red>["255.255.11.135"</font>, <font color=red>"255.255.111.35"]</font>. (Order does not matter) 


### code:

``` c


bool is_valid_ip(std::string str) {
    if (str.length() >= 2 && '0' == str[0]) {
        return false;
    }

    int i = atoi(str.c_str());
    if (i >= 0 && i <= 255) {
        return true;
    }

    return false;
}

bool is_valid_address(std::string str, int num, std::vector<string>& out_string) {
    if (str.empty()) {
        return false;
    }

    if (1 == num) {
        if (is_valid_ip(str)) {
            out_string.push_back(str);
            return true;
        } else {
            return false;
        }
    }

    int i = 1;
    while (i <= 3 && i < str.length()) {
        std::string s0 = str.substr(0, i);
        std::string s1 = str.substr(i, str.length() - i);
        std::vector<string> outString;
        if (is_valid_ip(s0) && is_valid_address(s1, num - 1, outString)) {
            for (auto it : outString) {
                out_string.push_back(s0 + "." + it);
            }
        }
        i++;
    }
    return true;
}


class Solution {
  public:
    vector<string> restoreIpAddresses(string s) {
        std::vector<string> out_string;
        is_valid_address(s, 4, out_string);
        return out_string;
    }
};


```