---
layout: post
title: [leetcode] -  Text Justification!
---


 Given an array of words and a length L, format the text such that each line has exactly L characters and is fully (left and right) justified.

You should pack your words in a greedy approach; that is, pack as many words as you can in each line. Pad extra spaces ' ' when necessary so 


that each line has exactly L characters.


Extra spaces between words should be distributed as evenly as possible. If the number of spaces on a line do not divide evenly between words, 

the empty slots on the left will be assigned more spaces than the slots on the right.


For the last line of text, it should be left justified and no extra space is inserted between words.


For example,

words: <font color=red>["This", "is", "an", "example", "of", "text", "justification."]</font>

L: <font color=red>16</font>. 

Return the formatted lines as:

----------
[

   "This    is    an",
   
   "example  of text",
   
   "justification.  "
   
]

----------

Note: Each word is guaranteed not to exceed L in length. 



Corner Cases:

    A line other than the last line might contain only one word. What should you do in this case?

    In this case, that line should be left-justified.


### code:

``` c
class Solution {
  public:
    int AllLenth(vector<string>& cur_lines) {
        int len = 0;
        for (int i = 0; i < cur_lines.size(); i++) {
            len += cur_lines[i].length();
        }
        return len;
    }

    string AdjustLines(vector<string>& cur_lines, int maxWidth, bool bLastLine) {
        string str_all = "";
        if (maxWidth == 0) {
            return str_all;
        }
        if (true == bLastLine) {
            for (int i = 0; i < cur_lines.size(); i++) {
                str_all += cur_lines[i];
                if (str_all.length() < maxWidth) {
                    str_all.append(1, ' ');
                }
            }
            str_all.append(maxWidth - str_all.length(), ' ');
            return str_all;
        } else if (cur_lines.size() == 1) {
            str_all = cur_lines[0];
            str_all.append(maxWidth - str_all.length(), ' ');
            return str_all;
        } else {
            str_all += cur_lines[cur_lines.size() - 1];
            int all_len = AllLenth(cur_lines);
            int empty_len = maxWidth - all_len;
            for (int i = (cur_lines.size() - 2); i >= 0; i--) {
                int len = empty_len / (i + 1);
                empty_len -= len;
                string empty;
                empty.append(len, ' ');
                str_all = empty + str_all;
                str_all = cur_lines[i] + str_all;
            }
        }
        return str_all;
    }

    vector<string> fullJustify(vector<string>& words, int maxWidth) {
        if (maxWidth == 0) {
            return words;
        }

        vector<string> cur_lines;
        vector<string> all_line;
        string cur_word = words[0];
        cur_lines.push_back(cur_word);
        int line_len = cur_word.length();
        for (int i = 1; i < words.size(); i++) {
            cur_word = words[i];

            // if < maxWidth
            if ((line_len + cur_word.length()) < maxWidth) {
                cur_lines.push_back(cur_word);
                line_len += cur_word.length() + 1;
            } else {
                all_line.push_back(AdjustLines(cur_lines, maxWidth, false));

                // new line
                cur_lines.clear();
                cur_lines.push_back(cur_word);
                line_len = cur_word.length();
            }
        }

        string str = AdjustLines(cur_lines, maxWidth, true);
        if (str.length() > 0) {
            all_line.push_back(str);
        }
        return all_line;
    }
};


```