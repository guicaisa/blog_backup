---
title: leetcode problem 39. 组合总和
tags: 算法
categories:
  - leetcode
  - leetcode 0001-0050
abbrlink: 9cdb587
date: 2021-06-14 21:50:26
---

# <center>leetcode problem 39. 组合总和</center>

## 链接

https://leetcode-cn.com/problems/combination-sum/



## 题目描述

给定一个无重复元素的数组 candidates 和一个目标数 target ，找出 candidates 中所有可以使数字和为 target 的组合。

candidates 中的数字可以无限制重复被选取。

说明：

所有数字（包括 target）都是正整数。
解集不能包含重复的组合。 
示例 1：

输入：candidates = \[2,3,6,7\], target = 7,
所求解集为：
\[
  \[7\],
  \[2,2,3\]
\]
示例 2：

输入：candidates = \[2,3,5\], target = 8,
所求解集为：
\[
  \[2,2,2,2\],
  \[2,3,3\],
  \[3,5\]
\]


提示：

1 <= candidates.length <= 30
1 <= candidates\[i\] <= 200
candidate 中的每个元素都是独一无二的。
1 <= target <= 500



## 解法

### 1.

递归回溯，遍历所有情况。

#### 代码

```c++
class Solution 
{
public:
    std::vector<std::vector<int> > CombinationSum(std::vector<int>& candidates, const int target) 
    {
        sort(candidates.begin(), candidates.end());
        std::vector<int> temp;
        std::vector<std::vector<int> > result;

        if (target == 0)
        {
            return result;
        }

        Recursive(candidates, target, 0, temp, result);

        return result;
    }

    void Recursive(const std::vector<int>& candidates, const int target, const int index, std::vector<int>& temp, std::vector<std::vector<int> >& result)
    {
        if (target == 0)
        {
            result.push_back(temp);
        }

        for (int i = index; i < candidates.size() && target - candidates[i] >= 0; ++i)
        {
            temp.push_back(candidates[i]);
            Recursive(candidates, target - candidates[i], i, temp, result);
            temp.pop_back();
        }
    }
};
```

