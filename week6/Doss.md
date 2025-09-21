# 25. Reverse Nodes in k-Group

Given the head of a linked list, reverse the nodes of the list k at a time, and return the modified list.

k is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of k then left-out nodes, in the end, should remain as it is.

You may not alter the values in the list's nodes, only nodes themselves may be changed.

## Solution

```cpp
ListNode* reverseKGroup(ListNode* head, int k) {
    auto ans = new ListNode(0, head);
    
    auto pre = ans;
    auto nxt = static_cast<ListNode*>(nullptr);

    auto cur = ans->next;
    auto stk = stack<ListNode*>();

    while(cur != nullptr) {
        stk.push(cur);

        if(stk.size() == k) {
            auto nxt = cur->next;

            while(stk.size() > 1) {
                auto top = stk.top();
                stk.pop();

                top->next = (stk.top()); 
            }

            auto lst = stk.top();
            stk.pop();

            lst->next = nxt;
            pre->next = cur;
            
            pre = lst;
            cur = lst;
        }

        cur = cur->next;
    }

    return ans->next;
}
```

# 29. Divide Two Integers

Given two integers dividend and divisor, divide two integers without using multiplication, division, and mod operator.

The integer division should truncate toward zero, which means losing its fractional part. For example, 8.345 would be truncated to 8, and -2.7335 would be truncated to -2.

Return the quotient after dividing dividend by divisor.

Note: Assume we are dealing with an environment that could only store integers within the 32-bit signed integer range: [−231, 231 − 1]. For this problem, if the quotient is strictly greater than 231 - 1, then return 231 - 1, and if the quotient is strictly less than -231, then return -231.

Example 1:

Input: dividend = 10, divisor = 3
Output: 3
Explanation: 10/3 = 3.33333.. which is truncated to 3.

Example 2:

Input: dividend = 7, divisor = -3
Output: -2
Explanation: 7/-3 = -2.33333.. which is truncated to -2.


## Solution 

```cpp
int divide(int dividend, int divisor) {
    if (dividend == INT_MIN && divisor == -1) {
        return INT_MAX;
    }
    
    auto negative = (dividend < 0) ^ (divisor < 0);
    
    auto absDividend = abs((long long)dividend);
    auto absDivisor = abs((long long)divisor);
    
    long long quotient = 0;
    
    while (absDividend >= absDivisor) {
        long long tempDivisor = absDivisor;
        long long multiple = 1;
        
        while (absDividend >= (tempDivisor << 1)) {
            tempDivisor <<= 1;
            multiple <<= 1;
        }
        
        absDividend -= tempDivisor;
        quotient += multiple;
    }
    
    if (negative) {
        quotient = -quotient;
    }
    
    return (int) max((long long)INT_MIN, min((long long)INT_MAX, quotient));
}
```

# 31. Next Permutation

A permutation of an array of integers is an arrangement of its members into a sequence or linear order.

    For example, for arr = [1,2,3], the following are all the permutations of arr: [1,2,3], [1,3,2], [2, 1, 3], [2, 3, 1], [3,1,2], [3,2,1].

The next permutation of an array of integers is the next lexicographically greater permutation of its integer. More formally, if all the permutations of the array are sorted in one container according to their lexicographical order, then the next permutation of that array is the permutation that follows it in the sorted container. If such arrangement is not possible, the array must be rearranged as the lowest possible order (i.e., sorted in ascending order).

    For example, the next permutation of arr = [1,2,3] is [1,3,2].
    Similarly, the next permutation of arr = [2,3,1] is [3,1,2].
    While the next permutation of arr = [3,2,1] is [1,2,3] because [3,2,1] does not have a lexicographical larger rearrangement.

Given an array of integers nums, find the next permutation of nums.

The replacement must be in place and use only constant extra memory.

Example 1:

Input: nums = [1,2,3]
Output: [1,3,2]

Example 2:

Input: nums = [3,2,1]
Output: [1,2,3]

Example 3:

Input: nums = [1,1,5]
Output: [1,5,1]

## Solution

```cpp
void nextPermutation(vector<int>& nums) {
    int n = nums.size();
    
    int pivot = -1;
    for (int i = n - 2; i >= 0; i--) {
        if (nums[i] < nums[i + 1]) {
            pivot = i;
            break;
        }
    }
    
    if (pivot == -1) {
        std::reverse(nums.begin(), nums.end());
        return;
    }
    
    int successor = -1;
    for (int i = n - 1; i > pivot; i--) {
        if (nums[i] > nums[pivot]) {
            successor = i;
            break;
        }
    }
    
    std::swap(nums[pivot], nums[successor]);
    
    std::reverse(nums.begin() + pivot + 1, nums.end());
}
```