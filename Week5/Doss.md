# 22. Generate Parentheses

Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

Example 1:

Input: n = 3
Output: ["((()))","(()())","(())()","()(())","()()()"]

Example 2:

Input: n = 1
Output: ["()"]

## Solution

```cpp
class Solution {
public:
    vector<string> box;
    
    vector<string> generateParenthesis(int n) {
        genParenthesis("", n, 0);
        return box;
    }

    void genParenthesis(string build, int open, int close) {
        if(open == 0 && close == 0) {
            box.push_back(build);
            return;
        }
        
        if(open != 0) {
            genParenthesis(build + "(", open - 1, close + 1);
        }

        if(close != 0) {
            genParenthesis(build + ")", open, close - 1);
        }
    }
};
```

# 23. Merge k Sorted Lists

You are given an array of k linked-lists lists, each linked-list is sorted in ascending order.

Merge all the linked-lists into one sorted linked-list and return it.

Example 1:

Input: lists = [[1,4,5],[1,3,4],[2,6]]
Output: [1,1,2,3,4,4,5,6]
Explanation: The linked-lists are:
[
  1->4->5,
  1->3->4,
  2->6
]
merging them into one sorted linked list:
1->1->2->3->4->4->5->6

Example 2:

Input: lists = []
Output: []

Example 3:

Input: lists = [[]]
Output: []

## Solution

```cpp
ListNode* mergeKLists(vector<ListNode*>& lists) {
    auto idx = vector<int>(20001, 0);

    for(auto node : lists) {
        while(node != nullptr) {
            idx[node->val + 10000] += 1;
            node = node->next;
        }
    }

    auto ans = new ListNode(0);
    auto head = ans;

    for(size_t i=0;i<idx.size();i++) {
        auto n = idx[i];

        for(auto k=0;k<n;k++) {
            head->next = new ListNode(i - 10000);
            head = head->next;
        }
    }

    return ans->next;
}
```