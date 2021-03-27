# LeetCode-19

题目链接：https://leetcode.com/problems/remove-nth-node-from-end-of-list/

## 算法思路

题目要在链表中删除倒数第$n$个节点，并且要在一次链表遍历中实现。即不能通过遍历链表先获得链表长度，再指定遍历长度。

假设有一个指针$p_1$指向链表的尾部空节点，同时有一个指针$p_2$指向需要删除的节点。那么保持两个指针的间隔为$n-1$，让$p_2$从链表头开始遍历，直到$p_1$指向空节点，则可以达到预想的指针状态，同时只需要一次遍历，满足题目要求。但是，链表删除节点需要知道删除节点的前驱节点，所以，我们加大指针间隔为$n$，使得当$p_1$到达尾部空节点，$p_2$指向删除节点的前驱节点。

接下来就是如何保证两个指针的间隔的问题。这里只需要让$p_1$先进行$n$次节点访问，然后再让$p_2$从头开始访问即可。

*实现细节*：需要注意边界情况，如空链表，单元素链表和删除节点为头节点。

## 代码

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        if (!head)  return head;

        ListNode *fast = head, *slow = head;
        while(n--) fast = fast->next;

        if (!fast) { // single element or deleting head
            slow = head->next;
            delete(head);
            head = slow;
        } else {
            while (fast->next) {
                fast = fast->next;
                slow = slow->next;
            }
            fast = slow->next->next;
            delete(slow->next);
            slow->next = fast;
        }
        
        return head;
    }
};
```

## 测试截图

![img](./accept.png)
