---
title: leetcode problem 24. 两两交换链表中的节点
tags: 算法
categories:
  - leetcode
  - leetcode 0001-0050
abbrlink: 40ffa434
date: 2021-04-18 14:40:28
---

# <center>leetcode problem 24. 两两交换链表中的节点</center>

## 链接

https://leetcode-cn.com/problems/swap-nodes-in-pairs/



## 题目描述

给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。

你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

![p24_1](leetcode-problem-24/p24_1.jpg)

示例 1：

输入：head = \[1,2,3,4\]
输出：\[2,1,4,3\]
示例 2：

输入：head = \[\]
输出：\[\]
示例 3：

输入：head = \[1\]
输出：\[1\]


提示：

链表中节点的数目在范围 \[0, 100\] 内
0 <= Node.val <= 100

进阶：你能在不修改链表节点值的情况下解决这个问题吗?（也就是说，仅修改节点本身。）



## 解法

### 1.

遍历的方式，使用3个节点变量，分别保存2个连续的指针，以及之后的第三个指针，交换2个连续指针的顺序，将交换顺序之后的结果连接在结果链表之后，然后从第三个指针开始继续遍历，遍历的结束条件为剩余的节点个数小于2。

#### 代码

```c++
struct ListNode 
{
    ListNode(int x) 
        : val(x), 
        next(NULL) 
    {
    }

    int val;
    ListNode* next;
};

class Solution 
{
public:
    ListNode* SwapPairs(ListNode* head) 
    {
        if (head == NULL || head->next == NULL)
        {
            return head;
        }

        ListNode* ret_head = NULL;
        ListNode* cur_node = NULL;

        while (head && head->next)
        {
            ListNode* first = head;
            ListNode* second = first->next;
            ListNode* third = second->next;

            first->next = NULL;
            second->next = first;

            if (ret_head == NULL)
            {
                ret_head = second;
                cur_node = second->next;
            }
            else
            {
                cur_node->next = second;
                cur_node = second->next;
            }

            head = third;
        }

        if (head)
        {
            cur_node->next = head;
        }

        return ret_head;
    }
};
```

### 2.

使用递归的方式，以2个节点为一组进行递归调用，将第二个节点的next节点作为头节点进行一次递归操作，交换2个节点的顺序，将返回的结果连接在交换顺序后的节点后面。

#### 代码

```c++
struct ListNode 
{
    ListNode(int x) 
        : val(x), 
        next(NULL) 
    {
    }

    int val;
    ListNode* next;
};

class Solution 
{
public:
    ListNode* SwapPairs(ListNode* head)
    {
        if (head == NULL || head->next == NULL)
        {
            return head;
        }

        ListNode* next = head->next;
        ListNode* next_two = head->next->next;
        ListNode* ret = this->SwapPairs(next_two);
        head->next = ret;
        next->next = head;

        return next;
    }
};
```

