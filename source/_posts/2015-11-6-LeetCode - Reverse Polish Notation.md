title: Reverse Polish Notation.
date:   2015-11-6 11:23:32
categories: algothrim
tags: algothrim
---


Evaluate the value of an arithmetic expression in Reverse Polish Notation.

Valid operators are +, -, *, /. Each operand may be an integer or another expression.

Some examples:

  ["2", "1", "+", "3", "*"] -> ((2 + 1) * 3) -> 9
  ["4", "13", "5", "/", "+"] -> (4 + (13 / 5)) -> 6


### code:
```cplusplus
class Solution
{
public:
    int evalRPN(vector<string>& tokens) {
        if(tokens.size() == 0)
            return 0;
            
        std::stack<int> stack;
        for(auto i: tokens)
        {

            if(string::npos != i.find("+") && i.length() == 1){
                int i_s = stack.top();
                stack.pop();
                int i_f = stack.top();
                stack.pop();
                stack.push(i_s + i_f);
            }else if(string::npos != i.find("-") && i.length() == 1){
                int i_s = stack.top();
                stack.pop();
                int i_f = stack.top();
                stack.pop();
                stack.push(i_f - i_s);
            }else if(string::npos != i.find("*") && i.length() == 1){
                int i_s = stack.top();
                stack.pop();
                int i_f = stack.top();
                stack.pop();
                stack.push(i_s * i_f);
            }else if(string::npos != i.find("/") && i.length() == 1){
                int i_s = stack.top();
                stack.pop();
                int i_f = stack.top();
                stack.pop();
                stack.push(i_f / i_s);
            }
            else{
                stack.push(atoi(i.c_str()));
            }
        }

        return stack.top();
    }
};
```