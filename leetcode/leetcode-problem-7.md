---
title: leetcode problem 7. 整数反转
tags: 算法
categories:
  - leetcode
  - leetcode 0001-0050
abbrlink: f10964c3
date: 2021-03-14 21:14:37
---

# <center>leetcode problem 7. 整数反转</center>

## 链接

https://leetcode-cn.com/problems/reverse-integer/



## 题目描述

给你一个 32 位的有符号整数 x ，返回将 x 中的数字部分反转后的结果。

如果反转后整数超过 32 位的有符号整数的范围 [−231,  231 − 1] ，就返回 0。

假设环境不允许存储 64 位整数（有符号或无符号）。


示例 1：

输入：x = 123
输出：321
示例 2：

输入：x = -123
输出：-321
示例 3：

输入：x = 120
输出：21
示例 4：

输入：x = 0
输出：0


提示：

-231 <= x <= 231 - 1



## 解法

### 1.

逆转数字位数，用数组保存逆序数字，用int64变量处理越界。

#### 代码

```c++
class Solution
{
public:
    int Reverse(int x) 
    {
        int64_t result = 0;
        std::vector<int> digit_vec;

        while (x)
        {
            digit_vec.push_back(x % 10);
            x /= 10;
        }

        for (size_t i = 0; i < digit_vec.size(); ++i)
        {
            result += std::pow(10, digit_vec.size() - 1 - i) * digit_vec[i];
        }

        if (result > (std::pow(2, 31) - 1) || result < (-1 * std::pow(2, 31)))
        {
            return 0;
        }

        return result;
    }
};
```

### 2.

优化代码，去除依赖数组和int64类型的操作，每遍历一次新的位数，就将原有结果乘10并加上新的余位，判断越界的方法为在每次计算出新值的时候提前判断。

定义result为当前的结果值，step为需要增加的余数，分为以下几种情况。

正数：

1：result > INT_MAX / 10，那么result * 10 必然越界。
2：result == INT_MAX / 10 && step > 7，INT_MAX = 2147483647，INT_MAX / 10 = 214748364，所以result == INT_MAX / 10 && step > 7 必然越界。

负数：

1：result < INT_MIN / 10，那么result * 10 必然越界。
2：result == INT_MIN / 10 && step < -8，INT_MIN = -2147483647 - 1，INT_MIN / 10 = -214748364，所以result == INT_MIN / 10 && step < -8 必越界。

#### 代码

```c++
class Solution
{
public:
    int Reverse(int x)
    {
        int result = 0;

        while (x)
        {
            int step = x % 10;

            if (result > INT_MAX / 10 || (result == INT_MAX / 10 && step > 7))
            {
                return 0;
            }

            if (result < INT_MIN / 10 || (result == INT_MIN / 10 && step < -8))
            {
                return 0;
            }

            result = result * 10 + (step);
            x /= 10;
        }

        return result;
    }
};
```

