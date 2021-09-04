---
title: leetcode problem 18. 四数之和
tags: 算法
categories:
  - leetcode
  - leetcode 0001-0050
abbrlink: 70a7dae8
date: 2021-04-06 23:09:36
---

# <center>leetcode problem 18. 四数之和</center>

## 链接

https://leetcode-cn.com/problems/4sum/



## 题目描述

给定一个包含 n 个整数的数组 nums 和一个目标值 target，判断 nums 中是否存在四个元素 a，b，c 和 d ，使得 a + b + c + d 的值与 target 相等？找出所有满足条件且不重复的四元组。

注意：答案中不可以包含重复的四元组。

 

示例 1：

输入：nums = [1,0,-1,0,-2,2], target = 0
输出：[[-2,-1,1,2],[-2,0,0,2],[-1,0,0,1]]
示例 2：

输入：nums = [], target = 0
输出：[]


提示：

0 <= nums.length <= 200
-109 <= nums[i] <= 109
-109 <= target <= 109



## 解法

### 1.

切割问题，分离成为一个数字和三个数字，使用3Sum的方式求剩下的三个数字的结果，与[3Sum的解法](https://guicaisa.github.io/posts/2b295c88.html)相似。

#### 代码

```c++
class Solution 
{
public:
    std::vector<std::vector<int> > FourSum(std::vector<int>& nums, const int target) 
    {
        std::vector<std::vector<int> > results;
        sort(nums.begin(), nums.end());

        for (size_t i = 0; i < nums.size(); ++i)
        {
            if (nums.size() - i < 4)
            {
                break;
            }

            if (i > 0 && nums[i] == nums[i - 1])
            {
                continue;
            }

            this->ThreeSum(nums, i + 1, target - nums[i], results);
        }

        return results;
    }

private:
    void ThreeSum(const std::vector<int>& nums, const int start_index, const int target, std::vector<std::vector<int> >& results)
    {
        int first_num = nums[start_index - 1];

        for (size_t i = start_index; i < nums.size(); ++i)
        {
            if (nums.size() - i < 3)
            {
                break;
            }

            if (i > start_index && nums[i] == nums[i - 1])
            {
                continue;
            }

            int left = i + 1;
            int right = nums.size() - 1;

            while (left < right)
            {
                int sum = nums[i] + nums[left] + nums[right];
                if (sum < target)
                {
                    ++left;
                }
                else if (sum > target)
                {
                    --right;
                }
                else
                {
                    std::vector<int> temp_vec;
                    temp_vec.emplace_back(first_num);
                    temp_vec.emplace_back(nums[i]);
                    temp_vec.emplace_back(nums[left]);
                    temp_vec.emplace_back(nums[right]);
                    results.push_back(temp_vec);

                    while (left + 1 < nums.size() && nums[left] == nums[left + 1])
                    {
                        ++left;
                    }

                    while (right > 0 && nums[right] == nums[right - 1])
                    {
                        --right;
                    }

                    ++left;
                    --right;
                }
            }
        }
    }
};
```

