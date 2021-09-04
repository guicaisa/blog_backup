---
title: leetcode problem 40. 组合总和 II
tags: 算法
categories:
  - leetcode
  - leetcode 0001-0050
abbrlink: c397e8d0
date: 2021-06-17 23:04:37
---

# <center>leetcode problem 40. 组合总和 II</center>

## 链接

https://leetcode-cn.com/problems/combination-sum-ii/



## 题目描述

给定一个数组 candidates 和一个目标数 target ，找出 candidates 中所有可以使数字和为 target 的组合。

candidates 中的每个数字在每个组合中只能使用一次。

说明：

所有数字（包括目标数）都是正整数。
解集不能包含重复的组合。 
示例 1:

输入: candidates = \[10,1,2,7,6,1,5\], target = 8,
所求解集为:
\[
  \[1, 7\],
  \[1, 2, 5\],
  \[2, 6\],
  \[1, 1, 6\]
\]
示例 2:

输入: candidates = \[2,5,2,1,2\], target = 5,
所求解集为:
\[
  \[1,2,2\],
  \[5\]
\]



## 解法

### 1.

与[组合总和](https://guicaisa.github.io/posts/9cdb587.html)类似，都是使用递归回溯的方式，但是由于每种组合中不能使用相同的数字(严格意义上来说是相同索引的数字，不同索引的相同数字可以使用)，所以递归传递参数的时候index必须为i+1，即下一轮递归从index之后的一个数字开始。去重操作体现在递归函数中，遍历阶段需要过滤掉相同的数字，因为index不同但是值相同的数字会在下几轮递归中使用，数组在最开始进行过排序，所以只需比遍历判断去重即可。

#### 代码

```c++
class Solution 
{
public:
	std::vector<std::vector<int> > CombinationSum2(std::vector<int>& candidates, const int target)
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

	void Recursive(const std::vector<int>& candidates, const int target, const int index, std::vector<int>& temp,
		std::vector<std::vector<int> >& result)
	{
		if (target == 0)
		{
			result.push_back(temp);
		}

		for (int i = index; i < candidates.size() && target - candidates[i] >= 0; ++i)
		{
			if (i > 0 && candidates[i] == candidates[i - 1] && i > index)
			{
				continue;
			}

			temp.push_back(candidates[i]);
			Recursive(candidates, target - candidates[i], i + 1, temp, result);
			temp.pop_back();
		}
	}
};
```

