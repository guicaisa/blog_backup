---
title: leetcode problem 30. 串联所有单词的子串
tags: 算法
categories:
  - leetcode
  - leetcode 0001-0050
abbrlink: 80f0518a
date: 2021-05-09 15:11:16
---

# <center>leetcode problem 30. 串联所有单词的子串</center>

## 链接

https://leetcode-cn.com/problems/substring-with-concatenation-of-all-words/



## 题目描述

给定一个字符串 s 和一些长度相同的单词 words。找出 s 中恰好可以由 words 中所有单词串联形成的子串的起始位置。

注意子串要与 words 中的单词完全匹配，中间不能有其他字符，但不需要考虑 words 中单词串联的顺序。

 

示例 1：

输入：
  s = \"barfoothefoobarman\",
  words = \[\"foo\",\"bar\"\]
输出：\[0,9\]
解释：
从索引 0 和 9 开始的子串分别是 \"barfoo\" 和 \"foobar\" 。
输出的顺序不重要, \[9,0\] 也是有效答案。
示例 2：

输入：
  s = \"wordgoodgoodgoodbestword\",
  words = \[\"word\",\"good\",\"best\",\"word\"\]
输出：\[\]



 ##  解法

### 1.

words中的所有字符串都是等长的，所以当我们在s中寻找串联的子串的时候，可以使用一个固定的步幅single_len来找到并处理每一个字符串，先使用一个map来保存words中所有字符串出现的次数。total_len代表words中所有字符串构成的子串的总长度，在遍历过程中，如果当前s字符串剩余的长度小于total_len时，可以直接跳出循环，避免后续无意义的遍历。

假设n为words数组的个数，遍历字符串s，以每个字符为起点，以single_len的长度获取s中的每一个字符串，使用临时的map来保存这些字符串，并且与之前的map中已保存的字符串出现的次数进行对比，如果当最终遍历完，获取的n个字符串与之前所保存的字符串个数一致，则表示找到了合适的子串起始位置，如不相符，则继续下一次循环处理。

#### 代码

```c++
class Solution
{
public:
    std::vector<int> FindSubString(const std::string& s, const std::vector<std::string>& words) 
    {
        std::vector<int> vi;

        if (s == "" || words.size() == 0)
        {
            return vi;
        }

        size_t single_len = words[0].size();
        size_t total_len = single_len * words.size();
        size_t s_len = s.size();
        std::unordered_map<std::string, int> words_map;

        for (size_t i = 0; i < words.size(); ++i)
        {
            words_map[words[i]] += 1;
        }

        for (size_t i = 0; i < s_len && s_len - i >= total_len; ++i)
        {
            bool flag = true;
            std::unordered_map<std::string, int> temp_words_map;
            for (size_t j = 0; j < words.size(); ++j)
            {
                std::string word = s.substr(i + j * single_len, single_len);
                if (temp_words_map[word] < words_map[word])
                {
                    temp_words_map[word] += 1;
                }
                else
                {
                    flag = false;
                    break;
                }
            }

            if (flag)
            {
                vi.emplace_back(i);
            }
        }

        return vi;
    }
};
```

