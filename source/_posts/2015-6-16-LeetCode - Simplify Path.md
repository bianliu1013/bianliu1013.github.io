---
layout: post
title: leetcode -  Remove Nth Node From End of List!
date:   2015-06-16 11:23:32
categories: leetcode c++
---


Given an absolute path for a file (Unix-style), simplify it.


For example,

path = <font color=red>"/home/"</font>, => <font color=red>"/home"</font>

path = <font color=red>"/a/./b/../../c/"</font>, => <font color=red>"/c"</font>


Corner Cases:


Did you consider the case where path = <font color=red>"/../"</font>?


In this case, you should return <font color=red>"/"</font>.


Another corner case is the path might contain multiple slashes <font color=red>'/'</font> together, such as <font color=red>"/home//foo/"</font>.
    

In this case, you should ignore redundant slashes and return <font color=red>"/home/foo"</font>.

### code:

``` c

class Solution {
  public:
    string simplifyPath(string path) {
        if (path.length() <= 1) {
            return path;
        }

        stack<string> s_stack;
        string str_left = path.substr(1, path.length() - 1);
        int pos = str_left.find('/');
        while (string::npos != pos) {
            if (pos == 0) {
                str_left = str_left.substr(pos + 1, str_left.length() - pos - 1);
                pos = str_left.find('/');
                continue;
            }

            string sub_str = str_left.substr(0, pos + 1);
            str_left = str_left.substr(pos + 1, str_left.length() - pos - 1);

            if (0 == sub_str.compare("../")) {
                if (s_stack.size() > 0) {
                    s_stack.pop();
                }
            } else if (0 == sub_str.compare("./")) {
                // do nothing
            } else if(sub_str.size() > 0) {
                s_stack.push(sub_str.substr(0, sub_str.length() - 1));
            }

            pos = str_left.find('/');
        }

        if (0 == str_left.compare(".")) {
        } else if (0 == str_left.compare("..")) {
            if(s_stack.size() > 0) s_stack.pop();
        } else if (str_left.length() > 0)
            s_stack.push(str_left.substr(0, str_left.length()));

        if(s_stack.size() == 0) {
            return "/";
        }


        std::string str_out;
        while(s_stack.size() > 0) {
            str_out = "/" + s_stack.top() + str_out;
            s_stack.pop();
        }
        return str_out;
    }
};

```