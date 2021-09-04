---
title: leetcode problem 16. 最接近的三数之和
tags: 算法
categories:
  - leetcode
  - leetcode 0001-0050
abbrlink: 952aa5bb
date: 2021-04-02 22:35:49
---

# <center>leetcode problem 16. 最接近的三数之和</center>

## 链接

https://leetcode-cn.com/problems/3sum-closest/



## 题目描述

给定一个包括 n 个整数的数组 nums 和 一个目标值 target。找出 nums 中的三个整数，使得它们的和与 target 最接近。返回这三个数的和。假定每组输入只存在唯一答案。

 

示例：

输入：nums = [-1,2,1,-4], target = 1
输出：2
解释：与 target 最接近的和是 2 (-1 + 2 + 1 = 2) 。


提示：

3 <= nums.length <= 10^3
-10^3 <= nums[i] <= 10^3
-10^4 <= target <= 10^4



## 解法

### 1.

解法与3sum类似，先排序数组，外层循环固定一个数字，然后在其之后的区间内使用双指针，从首尾向中间遍历，计算三个数字的和，获得与target差值的绝对值，如果等于0，直接返回这个和。使用一个变量保存最小差值和三个数的和，根据大小判断是否更新最小的差值和三数的和。如果三数之和小于target，因为数组是排序好的，后续遍历应该需要一个更大的值才有可能更接近target，左边索引向右移动，如果三数之和大于target，后续遍历应该需要一个更小的值，右边索引向左移动。外层循环和左右索引向中间递进的时候都需要去重，防止不必要的计算。

#### 代码

```c++
class Solution 
{
public:
    int ThreeSumClosest(std::vector<int>& nums, const int target) 
    {
        if (nums.size() == 0)
        {
            return 0;
        }

        sort(nums.begin(), nums.end());

        int min_diff = INT_MAX;
        int closest_sum = 0;

        for (size_t i = 0; i < nums.size(); ++i)
        {
            if (i > 0 && nums[i] == nums[i - 1])
            {
                continue;
            }

            int left = i + 1;
            int right = nums.size() - 1;

            while (left < right)
            {
                int sum = nums[i] + nums[left] + nums[right];
                int temp_diff = abs(sum - target);

                if (temp_diff == 0)
                {
                    return sum;
                }

                if (temp_diff < min_diff)
                {
                    closest_sum = sum;
                    min_diff = temp_diff;
                }

                if (sum < target)
                {
                    while (left + 1 < nums.size() && nums[left] == nums[left + 1])
                    {
                        ++left;
                    }
                    ++left;
                }
                else if (sum > target)
                {
                    while (right - 1 > i && nums[right] == nums[right - 1])
                    {
                        --right;
                    }
                    --right;
                }
            }
        }

        return closest_sum;
    }
};
```

