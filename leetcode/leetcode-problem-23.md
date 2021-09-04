---
title: leetcode problem 23. 合并K个升序链表
tags: 算法
categories:
  - leetcode
  - leetcode 0001-0050
abbrlink: 180ff31
date: 2021-04-17 20:46:39
---

# <center>leetcode problem 23. 合并K个升序链表</center>

## 链接

https://leetcode-cn.com/problems/merge-k-sorted-lists/



## 题目描述

给你一个链表数组，每个链表都已经按升序排列。

请你将所有链表合并到一个升序链表中，返回合并后的链表。

 

示例 1：

输入：lists = \[\[1,4,5\],\[1,3,4\],\[2,6\]\]
输出：\[1,1,2,3,4,4,5,6\]
解释：链表数组如下：
\[
  1->4->5,
  1->3->4,
  2->6
\]
将它们合并到一个有序链表中得到。
1->1->2->3->4->4->5->6
示例 2：

输入：lists = \[\]
输出：\[\]
示例 3：

输入：lists = \[\[\]\]
输出：\[\]


提示：

k == lists.length
0 <= k <= 10^4
0 <= lists\[i\].length <= 500
-10^4 <= lists\[i\]\[j\] <= 10^4
lists\[i\] 按 升序 排列
lists\[i\].length 的总和不超过 10^4



## 解法

### 1.

将节点的值域作为键，节点作为值，将所有节点放入multimap自动排序，然后按照顺序重新连接成新的链表。

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
    ListNode* MergeKLists(const std::vector<ListNode*>& lists) 
    {
        std::multimap<int, ListNode*> node_map;

        for (size_t i = 0; i < lists.size(); ++i)
        {
            ListNode* temp = lists[i];

            while (temp)
            {
                node_map.insert(std::make_pair(temp->val, temp));
                temp = temp->next;
            }
        }

        ListNode* head = NULL;
        ListNode* cur = NULL;

        if (node_map.size() > 0)
        {
            head = node_map.begin()->second;
            head->next = NULL;
            cur = head;
        }

        for (std::multimap<int, ListNode*>::iterator it = node_map.begin(); it != node_map.end(); ++it)
        {
            if (it == node_map.begin())
            {
                continue;
            }

            cur->next = it->second;
            cur = it->second;
            cur->next = NULL;
        }

        return head;
    }
};
```

### 2.

使用优先队列的方式，假设链表数组中有n个链表，我们将这n个链表的头节点以值域为键，节点为值的方式放入优先队列或者map中，自动进行排序，然后取出头部的一个节点，即值域最小的节点，放入新链表中，然后将该节点的next节点再次放入优先队列或者map中，不断从队列中取出头部的节点连入新的链表，直到所有链表遍历结束。

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
    ListNode* MergeKLists(const std::vector<ListNode*>& lists)
    {
        std::multimap<int, ListNode*> node_map;

        for (size_t i = 0; i < lists.size(); ++i)
        {
            if (lists[i])
            {
                node_map.insert(std::make_pair(lists[i]->val, lists[i]));
            }
        }

        ListNode* head = new ListNode(-1);
        ListNode* cur = head;

        while (node_map.size() > 0)
        {
            cur->next = node_map.begin()->second;
            node_map.erase(node_map.begin());
            cur = cur->next;
            if (cur->next)
            {
                node_map.insert(std::make_pair(cur->next->val, cur->next));
            }
        }

        return head->next;
    }
};
```

### 3.

使用分治策略，将数组不断二分进行递归，分别求解左右两部分的结果，递归调用返回之后，使用[MergeTwoLists](https://guicaisa.github.io/posts/1a0c2293.html)的算法合并2个链表。

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
    ListNode* MergeKLists(const std::vector<ListNode*>& lists)
    {
        if (lists.size() == 0)
        {
            return NULL;
        }

        return this->Recursive(lists, 0, lists.size() - 1);
    }

private:
    ListNode* Recursive(const std::vector<ListNode*>& lists, const int left, const int right)
    {
        if (left == right)
        {
            return lists[left];
        }

        if (left < right)
        {
            int mid = (left + right) / 2;
            ListNode* left_node = this->Recursive(lists, left, mid);
            ListNode* right_node = this->Recursive(lists, mid + 1, right);

            return this->MergeTwoLists(left_node, right_node);
        }

        return NULL;
    }

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

