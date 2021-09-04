---
title: leetcode problem 1. 两数之和
abbrlink: e6328cda
date: 2021-02-28 18:57:45
tags: 算法
categories: 
- leetcode
- leetcode 0001-0050
---

# <center>leetcode problem 1. 两数之和</center>

## 链接

https://leetcode-cn.com/problems/two-sum/



## 题目描述
给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 的那 两个 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素不能使用两遍。

你可以按任意顺序返回答案。

 

示例 1：

输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
示例 2：

输入：nums = [3,2,4], target = 6
输出：[1,2]
示例 3：

输入：nums = [3,3], target = 6
输出：[0,1]


提示：

2 <= nums.length <= 103
-109 <= nums[i] <= 109
-109 <= target <= 109
只会存在一个有效答案



## 解法
### 1.
第一次遍历，保存数组中的数字对应的下标到map中。第二次遍历，判断目标值与当前值的差是否在map中，获得其下标，获取的下标不能与当前正在遍历的下标相同，因为同一个元素不能使用两次。最后满足条件即可得出结果。

#### 代码
```c++
class Solution
{
public:
    std::vector<int> TwoSum(const std::vector<int>& nums, const int target)
    {
        std::map<int, int> number_to_index;

        for (size_t i = 0; i < nums.size(); ++i)
        {
            number_to_index[nums[i]] = i;
        }

        for (size_t i = 0; i < nums.size(); ++i)
        {
            int differ = target - nums[i];

            if (number_to_index.find(differ) != number_to_index.end() && i != number_to_index[differ])
            {
                std::vector<int> result;
                result.emplace_back(number_to_index[differ]);
                result.emplace_back(i);
                sort(result.begin(), result.end());

                return result;
            }
        }

        return std::vector<int>(0, 0);
    }
};
```

### 2.
只需遍历一次，保存数组中出现的数字对应的下标到map中，但是每次遍历时，检查map中是否有与当前值组合后符合目标值的数，由于题目描述已经确定只有一种解，找到匹配的结果直接返回即可。

#### 代码
```c++
class Solution
{
public:
    std::vector<int> TwoSum(const std::vector<int>& nums, const int target)
    {
        std::map<int, int> number_to_index;

        for (size_t i = 0; i < nums.size(); ++i)
        {
            int differ = target - nums[i];

            if (number_to_index.find(differ) != number_to_index.end())
            {
                std::vector<int> result;
                result.emplace_back(number_to_index[differ]);
                result.emplace_back(i);

                return result;
            }

            number_to_index[nums[i]] = i;
        }

        return std::vector<int>(0, 0);
    }
};
```