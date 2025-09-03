# Remove Nth Node From End of List

Given the head of a linked list, remove the nth node from the end of the list and return its head.

Example 1:

Input: head = [1,2,3,4,5], n = 2
Output: [1,2,3,5]

Example 2:

Input: head = [1], n = 1
Output: []

Example 3:

Input: head = [1,2], n = 1
Output: [1]

## Solution

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
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        auto dummy = new ListNode(0);
        dummy->next = head;
        
        auto fast = dummy;
        auto slow = dummy;
        
        for (int i = 0; i <= n; i++) {
            fast = fast->next;
        }
        
        while (fast != nullptr) {
            fast = fast->next;
            slow = slow->next;
        }
        
        auto nodeToDelete = slow->next;
        slow->next = slow->next->next;
        delete nodeToDelete;
        
        auto result = dummy->next;
        delete dummy;
        
        return result;
    }
};
```

# Valid Parentheses

Given a string s containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.
3. Every close bracket has a corresponding open bracket of the same type.

 

Example 1:

Input: s = "()"

Output: true

Example 2:

Input: s = "()[]{}"

Output: true

Example 3:

Input: s = "(]"

Output: false

Example 4:

Input: s = "([])"

Output: true

Example 5:

Input: s = "([)]"

Output: false

## Solution

```cpp
bool isValid(string s) {
    auto stk = std::stack<char>();

    for(auto c : s) {
        auto top = ' ';

        if(stk.empty() == false) top = stk.top();

        if(c == ')') {
            if(top != '(') return false;
            stk.pop();
            continue;
        }
        else if(c == '}') {
            if(top != '{') return false;
            stk.pop();
            continue;
        }
        else if(c == ']') {
            if(top != '[') return false;
            stk.pop();
            continue;
        }

        stk.push(c);
    }

    return stk.empty();
}
```

# Merge Two Sorted Lists

You are given the heads of two sorted linked lists list1 and list2.

Merge the two lists into one sorted list. The list should be made by splicing together the nodes of the first two lists.

Return the head of the merged linked list.
 
Example 1:

Input: list1 = [1,2,4], list2 = [1,3,4]
Output: [1,1,2,3,4,4]

Example 2:

Input: list1 = [], list2 = []
Output: []

Example 3:

Input: list1 = [], list2 = [0]
Output: [0]

## Solution

```cpp
ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
    auto up = list1;
    auto dw = list2;

    auto ans = new ListNode(0);
    auto cur = ans;

    while(up != nullptr || dw != nullptr) {
        auto u = 101, d = 101;

        if(up != nullptr) u = up->val;
        if(dw != nullptr) d = dw->val;

        if(u > d) {
            cur->next = new ListNode(d);
            cur = cur->next;
            dw = dw->next;
        }
        else {    
            cur->next = new ListNode(u);
            cur = cur->next;
            up = up->next;
        }
    }

    return ans->next;
}
```