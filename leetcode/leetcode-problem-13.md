---
title: leetcode problem 13. 罗马数字转整数
tags: 算法
categories:
  - leetcode
  - leetcode 0001-0050
abbrlink: d4fb0053
date: 2021-03-27 18:48:23
---

# <center>leetcode problem 13. 罗马数字转整数</center>

## 链接

https://leetcode-cn.com/problems/roman-to-integer/



## 题目描述

罗马数字包含以下七种字符: I， V， X， L，C，D 和 M。

字符          数值
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
例如， 罗马数字 2 写做 II ，即为两个并列的 1。12 写做 XII ，即为 X + II 。 27 写做  XXVII, 即为 XX + V + II 。

通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做 IIII，而是 IV。数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。同样地，数字 9 表示为 IX。这个特殊的规则只适用于以下六种情况：

I 可以放在 V (5) 和 X (10) 的左边，来表示 4 和 9。
X 可以放在 L (50) 和 C (100) 的左边，来表示 40 和 90。 
C 可以放在 D (500) 和 M (1000) 的左边，来表示 400 和 900。
给定一个罗马数字，将其转换成整数。输入确保在 1 到 3999 的范围内。

 

示例 1:

输入: \"III\"
输出: 3
示例 2:

输入: \"IV\"
输出: 4
示例 3:

输入: \"IX\"
输出: 9
示例 4:

输入: \"LVIII\"
输出: 58
解释: L = 50, V= 5, III = 3.
示例 5:

输入: \"MCMXCIV\"
输出: 1994
解释: M = 1000, CM = 900, XC = 90, IV = 4.


提示：

1 <= s.length <= 15
s 仅含字符 (\'I\', \'V\', \'X\', \'L\', \'C\', \'D\', \'M\')
题目数据保证 s 是一个有效的罗马数字，且表示整数在范围 [1, 3999] 内
题目所给测试用例皆符合罗马数字书写规则，不会出现跨位等情况。
IL 和 IM 这样的例子并不符合题目要求，49 应该写作 XLIX，999 应该写作 CMXCIX 。
关于罗马数字的详尽书写规则，可以参考 罗马数字 - Mathematics 。



## 解法

### 1.

倒序遍历字符串，并保存前一个遍历的字符，如果遇到数字1出现在对应的数字10或者5之前，需要减去对应的值，用来处理数字4和9的特殊情况，否则就增加。

#### 代码

```c++
class Solution
{
public:
    int RomanToInt(const std::string& s) 
    {
        std::unordered_map<char, int> roman_map;
        roman_map['I'] = 1;
        roman_map['V'] = 5;
        roman_map['X'] = 10;
        roman_map['L'] = 50;
        roman_map['C'] = 100;
        roman_map['D'] = 500;
        roman_map['M'] = 1000;

        int result = 0;
        char pre_char = 0;

        for (int i = s.size() - 1; i >= 0; --i)
        {
            int temp_value = roman_map[s[i]];
            if ((s[i] == 'I' && (pre_char == 'V' || pre_char == 'X')) ||
                (s[i] == 'X' && (pre_char == 'L' || pre_char == 'C')) ||
                (s[i] == 'C' && (pre_char == 'D' || pre_char == 'M')))
            {
                temp_value *= -1;
            }

            pre_char = s[i];
            result += temp_value;
        }

        return result;
    }
};
```

### 2.

简化代码。

#### 代码

```c++
class Solution
{
public:
    int RomanToInt(const std::string& s)
    {
        if (s == "")
        {
            return 0;
        }

        std::unordered_map<char, int> roman_map;
        roman_map['I'] = 1;
        roman_map['V'] = 5;
        roman_map['X'] = 10;
        roman_map['L'] = 50;
        roman_map['C'] = 100;
        roman_map['D'] = 500;
        roman_map['M'] = 1000;

        int result = roman_map[s.back()];
        for (int i = s.size() - 2; i >= 0; --i)
        {
            if (roman_map[s[i]] < roman_map[s[i + 1]])
            {
                result -= roman_map[s[i]];
            }
            else
            {
                result += roman_map[s[i]];
            }
        }

        return result;
    }
};
```



