# Leetcode 92. 反转链表 II （Intel 研发岗笔试题原型）

## 题目内容：

给你单链表的头指针 head 和两个整数`left`和`right`，其中`left <= right`。请你反转从位置`left`到位置`right`的链表节点，返回**反转后的链表**。
>### 示例 1：
>![](https://assets.leetcode.com/uploads/2021/02/19/rev2ex2.jpg)
>```
>输入：head = [1,2,3,4,5], left = 2, right = 4
>输出：[1,4,3,2,5]
>```
>### 示例 2：
>```
>输入：head = [5], left = 1, right = 1
>输出：[5]
>```

## 解题思路：
反转指定范围内的链表元素，我们需要先遍历到它的起始位置，然后再对链表进行反转操作。我们可以使用一次遍历实现反转。我们需要定义以下指针：
- `curr`：指向待反转区域的第一个节点`left`；
- `next`：永远指向`curr`的下一个节点，循环过程中,`curr`变化以后`next`会变化；
- `pre`：永远指向待反转区域的第一个节点`left`的前一个节点，在循环过程中不变。
接下来我们需要进行以下操作进行链表元素的反转：
1. 先将` curr `的下一个节点记录为` next`；
2. 把` curr `的下一个节点指向` next `的下一个节点；
3. 把` next `的下一个节点指向` pre `的下一个节点；
4. 把` pre `的下一个节点指向` next`。
5. 重复上述操作直至所有范围内的节点值完成反转

## 代码：
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode *reverseBetween(ListNode *head, int left, int right) {
        ListNode *dummyNode = new ListNode(-1);
        dummyNode->next = head;
        ListNode *pre = dummyNode;
        for (int i = 0; i < left - 1; i++) {
            pre = pre->next;
        }
        ListNode *cur = pre->next;
        ListNode *next;
        for (int i = 0; i < right - left; i++) {
            next = cur->next;
            cur->next = next->next;
            next->next = pre->next;
            pre->next = next;
        }
        return dummyNode->next;
    }
};
```
## 变体问题（Intel研发岗在线笔试）：
除了需要反转指定范围的链表，还需要删掉范围内指定值的元素，那么我们再将删除链表元素的方法补充进去即可。
## 代码：
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode *removeElements(ListNode *head, int key) {
	    if (head == nullptr)
		    return nullptr;
	    if (head != nullptr && head->val == key)
		    head = head->next;
	    ListNode* prev = head;
	    while (prev->next != nullptr) {
		    if (prev->next->val == key){
			    ListNode* p = prev->next;
			    prev->next = prev->next->next;
		    } else 
			    prev = prev->next;
	    }
	    return head;
    }

    ListNode *reverseAndDelete(ListNode *head, int left, int right,int key) {
        ListNode *dummyNode = new ListNode(-1);
        dummyNode->next = head;
        ListNode *pre = dummyNode;
        for (int i = 0; i < left - 1; i++) {
            pre = pre->next;
        }
        ListNode *cur = pre->next;
        ListNode *next;
        for (int i = 0; i < right - left; i++) {
            next = cur->next;
            cur->next = next->next;
            next->next = pre->next;
            pre->next = next;
        }
        ListNode *ans = removeElements(dummyNode->next,key);
        return ans;
    }
};