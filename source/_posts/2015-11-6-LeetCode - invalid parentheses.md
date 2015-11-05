---
layout: post
title: leetcode -  invalid parentheses
date:   2015-11-6 11:23:32
categories: leetcode c++
---

Remove the minimum number of invalid parentheses in order to make the input string valid. Return all possible results.

Note: The input string may contain letters other than the parentheses ( and ).

Examples:
---
"()())()" -> ["()()()", "(())()"]
"(a)())()" -> ["(a)()()", "(a())()"]
")(" -> [""]

---

### code:
<pre><code>
class Solution {
    int sum_all;
  public:
    vector<string> removeInvalidParentheses(string s) {
        vector<string> results;

        sum_all = sum(s);
        string cur_result = "";
        removeDsf(s, 0, cur_result, results);
        sort(results.begin(), results.end());
        results.erase(unique(results.begin(), results.end()), results.end());

        if (results.size() == 0 && s.length() > 0) {
            results.push_back("");
        }
        return results;
    }

    int sum(string str) {
        int c = 0;
        int sum = 0;
        for (auto i : str) {
            if (i == '(') {
                c++;
            } else if (i == ')') {
                c--;
                if (c == -1) {
                    c = 0;
                } else if (c >= 0) {
                    sum++;
                }
            }
        }
        return sum;
    }

    bool invalid(string cur_result) {
        int c = 0;
        for (auto i : cur_result) {
            if (i == '(') {
                c++;
            } else if (i == ')') {
                c--;
            }
            if ( c < 0) {
                return false;
            }
        }
        return c == 0 && sum(cur_result) == sum_all;
    }

    void removeDsf(string s, int l_count, string& cur_result, vector<string>& results) {
        if (l_count < 0) {
            return;
        }

        if (s.length() == 0) {
            if (invalid(cur_result)) {
                results.push_back(cur_result);
            }
            return;
        }
        

        if (s.at(0) == '(' || s.at(0) == ')') {
            std::string s0 = s.substr(1, s.length() - 1);
            string new_result = cur_result;
            removeDsf(s0,  l_count, new_result, results);

            l_count += s.at(0) == '(' ? 1 : -1;
            new_result = cur_result + s.at(0);
            removeDsf(s0, l_count, new_result, results);
        } else {
            cur_result += s.at(0);
            std::string s0 = s.substr(1, s.length() - 1);
            removeDsf(s0, l_count, cur_result, results);
        }
    }
};
</code></pre>