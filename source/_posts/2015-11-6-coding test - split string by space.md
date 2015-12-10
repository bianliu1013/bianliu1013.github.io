title: split string by space.
date:   2015-11-6 15:23:32
categories: algothrim
tags: algothrim
---


didn't do well in coding test this morning. write below string utility. go fight!


### code:
```cplusplus
#include <string>
#include <sstream>
#include <iostream>
#include <vector>

void trim_string(std::string& input_string){
    while(!input_string.empty() && ' ' == input_string.at(0)){
        input_string.erase(0, 1);
    }
}

void splict_string(std::string input, std::vector<std::string>& v)
{
    std::string input_string = input;
    trim_string(input_string);

    size_t offset = 0;
    offset = input_string.find(' ');
    while(std::string::npos != offset)
    {
        
        std::string sub = input_string.substr(0, offset);
        v.push_back(sub);
        
        input_string.erase(0, offset);
        trim_string(input_string);
        offset = input_string.find(' ');
    }

    if (input_string.length() > 0)
    {
        v.push_back(input_string);
        /* code */
    }
}
```