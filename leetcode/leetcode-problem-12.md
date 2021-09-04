---
title: leetcode problem 12. 整数转罗马数字
tags: 算法
categories:
  - leetcode
  - leetcode 0001-0050
abbrlink: 7c642ae4
date: 2021-03-25 22:41:29
---

# <center>leetcode problem 12. 整数转罗马数字</center>

## 链接

https://leetcode-cn.com/problems/integer-to-roman/



## 题目描述

罗马数字包含以下七种字符： I， V， X， L，C，D 和 M。

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
给定一个整数，将其转为罗马数字。输入确保在 1 到 3999 的范围内。

 

示例 1:

输入: 3
输出: \"III\"
示例 2:

输入: 4
输出: \"IV\"
示例 3:

输入: 9
输出: \"IX\"
示例 4:

输入: 58
输出: \"LVIII\"
解释: L = 50, V = 5, III = 3.
示例 5:

输入: 1994
输出: \"MCMXCIV\"
解释: M = 1000, CM = 900, XC = 90, IV = 4.


提示：

1 <= num <= 3999



## 解法

### 1.

单独处理4，5，9三个数字的情况，小于4和大于5的数字，有统一的处理方式，数字1-3根据个数使用对应的数字1累加，数字6-8根据个数在对应的数字5后面累加上相应个数的1，我们需要根据当前的个，十，百，千的位来选择对应的罗马数字。

#### 代码

```c++
class Solution
{
public:
    std::string IntToRoman(int num)
    {
        int step = 1;
        std::string result;

        while (num)
        {
            int step_num = num % 10;
            std::string temp_str;

            if (step == 1)
            {
                switch (step_num)
                {
                    case 9:
                        temp_str = "IX";
                        break;
                    case 4:
                        temp_str = "IV";
                        break;
                    case 5:
                        temp_str = "V";
                        break;
                    default:
                        if (step_num > 5)
                        {
                            temp_str = "V" + std::string(step_num - 5, 'I');
                        }
                        else if (step_num < 5)
                        {
                            temp_str = std::string(step_num, 'I');
                        }
                        break;
                }
            }
            else if (step == 2)
            {
                switch (step_num)
                {
                    case 9:
                        temp_str = "XC";
                        break;
                    case 4:
                        temp_str = "XL";
                        break;
                    case 5:
                        temp_str = "L";
                        break;
                    default:
                        if (step_num > 5)
                        {
                            temp_str = "L" + std::string(step_num - 5, 'X');
                        }
                        else if (step_num < 5)
                        {
                            temp_str = std::string(step_num, 'X');
                        }
                        break;
                }
            }
            else if (step == 3)
            {
                switch (step_num)
                {
                    case 9:
                        temp_str = "CM";
                        break;
                    case 4:
                        temp_str = "CD";
                        break;
                    case 5:
                        temp_str = "D";
                        break;
                    default:
                        if (step_num > 5)
                        {
                            temp_str = "D" + std::string(step_num - 5, 'C');
                        }
                        else if (step_num < 5)
                        {
                            temp_str = std::string(step_num, 'C');
                        }
                        break;
                }
            }
            else if (step == 4)
            {
                temp_str = std::string(step_num, 'M');
            }

            result = temp_str + result;
            num /= 10;
            ++step;
        }

        return result;
    }
};
```

### 2.

排列出数字和罗马字符对应的数组，特别列出1，4，5，9相关的数字对应的字符，遍历数组，如果输入的数字大于等于当前的数字，就将其减去，并将相应的罗马字符添加到结果中。数字4，5，9已经被特殊处理，剩余的1，2，3，6，7，8只需要用相应的数字1来堆积就可以了。

#### 代码

```c++
class Solution
{
public:
    std::string IntToRoman(int num)
    {
        std::string result;
        std::vector<int> vi = { 1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1 };
        std::vector<std::string> vr = { "M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I" };

        for (size_t i = 0; i < vi.size(); ++i)
        {
            while (num >= vi[i])
            {
                num -= vi[i];
                result += vr[i];
            }
        }

        return result;
    }
};
```

