---
title: leetcode problem 21. 合并两个有序链表
tags: 算法
categories:
  - leetcode
  - leetcode 0001-0050
abbrlink: 1a0c2293
date: 2021-04-12 23:06:23
---

# <center>leetcode problem 21. 合并两个有序链表</center>

## 链接

https://leetcode-cn.com/problems/merge-two-sorted-lists/



## 题目描述

将两个升序链表合并为一个新的 升序 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

 ![p21_1](leetcode-problem-21/p21_1.jpg)

示例 1：


输入：l1 = [1,2,4], l2 = [1,3,4]
输出：[1,1,2,3,4,4]
示例 2：

输入：l1 = [], l2 = []
输出：[]
示例 3：

输入：l1 = [], l2 = [0]
输出：[0]


提示：

两个链表的节点数目范围是 [0, 50]
-100 <= Node.val <= 100
l1 和 l2 均按 非递减顺序 排列



## 解法

### 1.

常规遍历的方法。使用一个头指针保存链表起始的指针，方便返回结果。然后开始遍历l1和l2，比较2个指针值域的大小，从而决定指针的链接顺序，值域较小的指针先行递进，再进行下一次遍历。如果一个链表遍历到底了，就直接将还没遍历完的另一个链表直接连接再当前链表的后面，最后由头指针的next返回结果。

### 代码

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
    ListNode* MergeTwoLists(ListNode* l1, ListNode* l2)
    {
        ListNode* head = new ListNode(-1);
        ListNode* cur = head;

        while (l1 && l2)
        {
            if (l1->val < l2->val)
            {
                cur->next = l1;
                l1 = l1->next;
            }
            else
            {
                cur->next = l2;
                l2 = l2->next;
            }

            cur = cur->next;
        }

        cur->next = l1 ? l1 : l2;

        return head->next;
    }
};
```

### 2.

使用递归的方法，每层递归仍然使用一个头指针，方便返回结果。核心思想是如果在l1和l2都不为空的情况下，比较l1和l2的值域大小，较小的那一个指针，向下递进一步，将l1->next和l2或者l1和l2->next作为下一级递归函数的变量，不断的递归调用，求出最终的结果，当然也要考虑l1和l2同时为空或者其中有一个为空的特殊情况。

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
    ListNode* MergeTwoLists(ListNode* l1, ListNode* l2)
    {
        return this->Recursive(l1, l2);
    }

private:
    ListNode* Recursive(ListNode* l1, ListNode* l2)
    {
        ListNode* head = new ListNode(-1);

        if (l1 == NULL && l2 == NULL)
        {
            head->next = NULL;
        }
        else if (l1 == NULL)
        {
            head->next = l2;
        }
        else if (l2 == NULL)
        {
            head->next = l1;
        }
        else
        {
            if (l1->val < l2->val)
            {
                head->next = l1;
                head->next->next = this->Recursive(l1->next, l2);
            }
            else
            {
                head->next = l2;
                head->next->next = this->Recursive(l1, l2->next);
            }
        }

        return head->next;
    }
};
```

