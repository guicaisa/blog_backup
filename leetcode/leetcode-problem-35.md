---
title: leetcode problem 35. 搜索插入位置
tags: 算法
categories:
  - leetcode
  - leetcode 0001-0050
abbrlink: 787fa326
date: 2021-05-31 23:44:03
---

# <center>leetcode problem 35. 搜索插入位置</center>

## 链接

https://leetcode-cn.com/problems/search-insert-position/



## 题目描述

插入的位置。

你可以假设数组中无重复元素。

示例 1:

输入: \[1,3,5,6\], 5
输出: 2
示例 2:

输入: \[1,3,5,6\], 2
输出: 1
示例 3:

输入: \[1,3,5,6\], 7
输出: 4
示例 4:

输入: \[1,3,5,6\], 0
输出: 0



## 解法

### 1.

解法就是简单的二分查找，当二分查找的循环退出的时候，如果找不到目标值，那左边界就是目标值的插入点。

#### 代码

```c++
class Solution 
{
public:
    int SearchInsert(const std::vector<int>& nums, const int target) 
    {
        int left = 0;
        int right = nums.size() - 1;
        int mid = 0;

        while (left <= right)
        {
            mid = (left + right) / 2;

            if (nums[mid] > target)
            {
                right = mid - 1;
            }
            else if (nums[mid] < target)
            {
                left = mid + 1;
            }
            else
            {
                return mid;
            }
        }

        return left;
    }
};
```

