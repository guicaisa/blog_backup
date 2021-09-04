---
title: leetcode problem 31. 下一个排列
tags: 算法
categories:
  - leetcode
  - leetcode 0001-0050
abbrlink: 9661bf26
date: 2021-05-20 22:59:41
---

# <center>leetcode problem 31. 下一个排列</center>

## 链接

https://leetcode-cn.com/problems/next-permutation/



## 题目描述

实现获取 下一个排列 的函数，算法需要将给定数字序列重新排列成字典序中下一个更大的排列。

如果不存在下一个更大的排列，则将数字重新排列成最小的排列（即升序排列）。

必须 原地 修改，只允许使用额外常数空间。

 

示例 1：

输入：nums = \[1,2,3\]
输出：\[1,3,2\]
示例 2：

输入：nums = \[3,2,1\]
输出：\[1,2,3\]
示例 3：

输入：nums = \[1,1,5\]
输出：\[1,5,1\]
示例 4：

输入：nums = \[1\]
输出：\[1\]


提示：

1 <= nums.length <= 100
0 <= nums\[i\] <= 100



## 解法

### 1.

根据肉眼观察，寻找下一个数字的排列，需要从数组的右边开始寻找第一组是升序形式的2个数字，将其进行交换才能使得下一次的数字排列有可能是更大的，比如1，2，3，4，从右边开始找到的第一个升序排列的2个数字是3，4，交换3和4获得下一个数字排列1，2，4，3。

如果是另一种复杂的情况，比如1，4，3，2，从右开始找到第一组升序排列的数字是1，4，这里是不能直接交换1和4的位置，因为4后面还有其他比4小的数字，如果是4，1，3，2的话明显结果是不对的，因为中间还有很多次大一点的排列没有列举出来。遇到这种情况，应该从尾遍历到1之前的所有数字，遇到第一个比1大的数字，就交换这个数字和1的位置上，保证了下一个数字排列的正确性，再将后续的3个数字的顺序倒置，保证后面3个数字的排列顺序是最小的原始的升序状态。

#### 代码

```c++
class Solution
{
public:
    void NextPermutation(std::vector<int>& nums)
    {
        if (nums.size() == 0)
        {
            return;
        }

        for (int i = nums.size() - 2; i >= 0; --i)
        {
            if (nums[i] < nums[i + 1])
            {
                for (int j = nums.size() - 1; j > i; --j)
                {
                    if (nums[j] > nums[i])
                    {
                        int temp = nums[j];
                        nums[j] = nums[i];
                        nums[i] = temp;
                        std::reverse(nums.begin() + i + 1, nums.end());
                        return;
                    }
                }
            }
        }

        std::reverse(nums.begin(), nums.end());
    }
};
```



