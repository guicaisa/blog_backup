---
title: leetcode problem 29. 两数相除
tags: 算法
categories:
  - leetcode
  - leetcode 0001-0050
abbrlink: fde7182e
date: 2021-05-07 22:38:02
---

# <center>leetcode problem 29. 两数相除</center>

## 链接

https://leetcode-cn.com/problems/divide-two-integers/



## 题目描述

给定两个整数，被除数 dividend 和除数 divisor。将两数相除，要求不使用乘法、除法和 mod 运算符。

返回被除数 dividend 除以除数 divisor 得到的商。

整数除法的结果应当截去（truncate）其小数部分，例如：truncate(8.345) = 8 以及 truncate(-2.7335) = -2

 

示例 1:

输入: dividend = 10, divisor = 3
输出: 3
解释: 10/3 = truncate(3.33333..) = truncate(3) = 3
示例 2:

输入: dividend = 7, divisor = -3
输出: -2
解释: 7/-3 = truncate(-2.33333..) = -2


提示：

被除数和除数均为 32 位有符号整数。
除数不为 0。
假设我们的环境只能存储 32 位有符号整数，其数值范围是 \[−2^31,  2^31 − 1\]。本题中，如果除法结果溢出，则返回 2^31 − 1。



## 解法

### 1.

一直用被除数减去除数，直到小于除数位置，统计其次数，就是最后的商。如果一直循环减的话，效率比较低，在主循环中再套一层循环，除数不断翻倍，扩大每次减去的值，减少循环的次数，提升效率。

#### 代码

```c++
class Solution 
{
public:
	int Divide(int dividend, int divisor)
	{
		if (dividend == INT_MIN && divisor == -1)
		{
			return INT_MAX;
		}

		bool symbol_flag = false;

		if ((dividend < 0 && divisor > 0) || (dividend > 0 && divisor < 0))
		{
			symbol_flag = true;
		}

		long abs_dividend = labs(dividend);
		long abs_divisor = labs(divisor);
		long count = 0;

		while (abs_dividend >= abs_divisor)
		{
			long temp = abs_divisor;
			long num = 1;

			while ((temp << 1) <= abs_dividend)
			{
				temp <<= 1;
				num <<= 1;
			}

			abs_dividend -= temp;
			count += num;
		}

		return symbol_flag ? -1 * count : count;
	}
};
```

