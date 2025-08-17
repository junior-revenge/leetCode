# Median of Two Sorted Arrays

## Description

Given two sorted arrays nums1 and nums2 of size m and n respectively, return the median of the two sorted arrays.

The overall run time complexity should be O(log (m+n)).

Example 1:

Input: nums1 = [1,3], nums2 = [2]
Output: 2.00000
Explanation: merged array = [1,2,3] and median is 2.

Example 2:

Input: nums1 = [1,2], nums2 = [3,4]
Output: 2.50000
Explanation: merged array = [1,2,3,4] and median is (2 + 3) / 2 = 2.5.

## Solution

```cpp
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        nums1.insert(nums1.end(), nums2.begin(), nums2.end());
        sort(nums1.begin(), nums1.end());

        auto siz = nums1.size();
        auto mid = siz / 2;

        if(siz % 2 == 0) {
            double l = nums1[mid-1];
            double r = nums1[mid];

            return static_cast<double>((l + r) / 2);
        }
        
        return static_cast<double>(nums1[mid]);
    }
};
```

# Longest Substring Without Repeating Characters

## Description

Given a string s, find the length of the longest

without duplicate characters.

Example 1:

Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.

Example 2:

Input: s = "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.

Example 3:

Input: s = "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.

## Solution

```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        auto box = std::set<char>();
        auto l = 0;
        auto mx = 0;

        for(size_t r = 0; r<s.size();r++) {
            while(box.find(s[r]) != box.end()) {
                box.erase(s[l]);
                l++;
            }

            box.insert(s[r]);

            auto len = r - l + 1;
            mx = mx > len ? mx : len;
        }

        return mx;
    }
};
```

# Add Two Numbers

## Description

You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

Example 1:

Input: l1 = [2,4,3], l2 = [5,6,4]
Output: [7,0,8]
Explanation: 342 + 465 = 807.

Example 2:

Input: l1 = [0], l2 = [0]
Output: [0]

Example 3:

Input: l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
Output: [8,9,9,9,0,0,0,1]

## Solution

```cpp
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        auto origin = new ListNode();
        auto node = origin;
        auto carry = false;

        while(l1 != nullptr || l2 != nullptr || carry) {
            auto one = l1 == nullptr ? 0 : l1->val;
            auto two = l2 == nullptr ? 0 : l2->val;

            auto sum = one + two;
            sum = carry ? sum + 1 : sum;

            node->val = sum > 9 ? sum - 10 : sum;

            carry = sum > 9;

            if(l1 != nullptr) l1 = l1->next;
            if(l2 != nullptr) l2 = l2->next;

            if(l1 != nullptr || l2 != nullptr || carry) node->next = new ListNode();
            node = node->next;

            std::cout<<sum<<std::endl;
        }

        return origin;
    }
};
```