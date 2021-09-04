---
title: leetcode problem 2. 两数相加
abbrlink: ec175825
date: 2021-03-01 21:13:28
tags: 算法
categories:
- leetcode
- leetcode 0001-0050
---

# <center>leetcode problem 2. 两数相加</center>

## 链接

https://leetcode-cn.com/problems/add-two-numbers/



## 题目描述
给你两个 非空 的链表，表示两个非负的整数。它们每位数字都是按照 逆序 的方式存储的，并且每个节点只能存储 一位 数字。

请你将两个数相加，并以相同形式返回一个表示和的链表。

你可以假设除了数字 0 之外，这两个数都不会以 0 开头。



示例 1：

 ![](leetcode-problem-2/addtwonumber1.jpg)

输入：l1 = [2,4,3], l2 = [5,6,4]
输出：[7,0,8]
解释：342 + 465 = 807.
示例 2：

输入：l1 = [0], l2 = [0]
输出：[0]
示例 3：

输入：l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
输出：[8,9,9,9,0,0,0,1]


提示：

每个链表中的节点数在范围 [1, 100] 内
0 <= Node.val <= 9
题目数据保证列表表示的数字不含前导零



## 解法
### 1.
同时遍历2个链表，将值域进行相加，超过10的部分需要进位，保留到下一个节点的计算中，将余数部分保存在新节点的值域中，添加到结果链表的最后，直到2个链表都遍历完毕，获得结果。

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
    ListNode* AddTwoNumbers(const ListNode* l1, const ListNode* l2)
    {
        if (l1 == NULL && l2 == NULL)
        {
            return new ListNode(0);
        }

        ListNode* head = NULL;
        ListNode* cur_node = NULL;
        int carry_add = 0;

        while (l1 || l2)
        {
            if (cur_node == NULL)
            {
                cur_node = new ListNode(0);
                head = cur_node;
            }
            else
            {
                cur_node->next = new ListNode(0);
                cur_node = cur_node->next;
            }

            int sum = 0;

            if (l1 && l2)
            {
                sum = l1->val + l2->val + carry_add;
                l1 = l1->next;
                l2 = l2->next;
            }
            else if (l1 && l2 == NULL)
            {
                sum = l1->val + carry_add;
                l1 = l1->next;
            }
            else if (l2 && l1 == NULL)
            {
                sum = l2->val + carry_add;
                l2 = l2->next;
            }

            cur_node->val = sum % 10;
            carry_add = sum / 10;
        }

        if (carry_add != 0)
        {
            cur_node->next = new ListNode(carry_add);
        }

        return head;
    }
};
```

### 2.
创建一个虚拟的头节点，简化代码结构和流程。

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
    ListNode* AddTwoNumbers(const ListNode* l1, const ListNode* l2)
    {
        ListNode head(0);
        ListNode* cur_node = &head;
        int carry_add = 0;

        while (l1 || l2 || carry_add)
        {
            int sum = (l1 ? l1->val : 0) + (l2 ? l2->val : 0) + carry_add;

            cur_node->next = new ListNode(sum % 10);
            cur_node = cur_node->next;
            carry_add = sum / 10;

            if (l1)
            {
                l1 = l1->next;
            }

            if (l2)
            {
                l2 = l2->next;
            }
        }

        return head.next;
    }
};
```