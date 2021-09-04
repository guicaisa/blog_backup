---
title: leetcode problem 20. 有效的括号
tags: 算法
categories:
  - leetcode
  - leetcode 0001-0050
abbrlink: b140cbff
date: 2021-04-08 21:56:39
---

# <center>leetcode problem 20. 有效的括号</center>

## 链接

https://leetcode-cn.com/problems/valid-parentheses/



## 题目描述

给定一个只包括 \'(\'，\')\'，\'{\'，\'}\'，\'[\'，\']\' 的字符串 s ，判断字符串是否有效。

有效字符串需满足：

左括号必须用相同类型的右括号闭合。
左括号必须以正确的顺序闭合。


示例 1：

输入：s = \"()\"
输出：true
示例 2：

输入：s = \"()[]{}\"
输出：true
示例 3：

输入：s = \"(]\"
输出：false
示例 4：

输入：s = \"([)]\"
输出：false
示例 5：

输入：s = \"{[]}\"
输出：true


提示：

1 <= s.length <= 104
s 仅由括号 \'()[]{}\' 组成



## 解法

### 1.

由于这个题目中的括号不只包含小括号，还包括中括号和大括号，所以如果单单使用几个变量来记录左括号的值，碰到右括号再使对应的变量-1，这种方法是不适用的，因为没有考虑到三种括号位置的先后关系。所以使用栈的方式来处理这种问题是最合适，本身就包含每个元素的前后位置信息。遍历字符串，遇到左括号入栈，遇到右括号时查找栈顶元素，如果是空栈或者不是对应的左括号则不是有效的括号。

#### 代码

```c++
class Solution 
{
public:
    bool IsValid(const std::string& s)
    {
        std::stack<char> char_st;

        for (size_t i = 0; i < s.size(); ++i)
        {
            switch (s[i])
            {
                case '(':
                case '[':
                case '{':
                    char_st.push(s[i]);
                    break;
                case ')':
                    if (char_st.size() == 0 || char_st.top() != '(')
                    {
                        return false;
                    }
                    else
                    {
                        char_st.pop();
                    }
                    break;
                case ']':
                    if (char_st.size() == 0 || char_st.top() != '[')
                    {
                        return false;
                    }
                    else
                    {
                        char_st.pop();
                    }
                    break;
                case '}':
                    if (char_st.size() == 0 || char_st.top() != '{')
                    {
                        return false;
                    }
                    else
                    {
                        char_st.pop();
                    }
                    break;
                default:
                    break;
            }
        }

        return char_st.size() == 0;
    }
};
```

