# 30. Substring with Concatenation of All Words

You are given a string s and an array of strings words. All the strings of words are of the same length.

A concatenated string is a string that exactly contains all the strings of any permutation of words concatenated.

    For example, if words = ["ab","cd","ef"], then "abcdef", "abefcd", "cdabef", "cdefab", "efabcd", and "efcdab" are all concatenated strings. "acdbef" is not a concatenated string because it is not the concatenation of any permutation of words.

Return an array of the starting indices of all the concatenated substrings in s. You can return the answer in any order.

Example 1:

Input: s = "barfoothefoobarman", words = ["foo","bar"]

Output: [0,9]

Explanation:

The substring starting at 0 is "barfoo". It is the concatenation of ["bar","foo"] which is a permutation of words.
The substring starting at 9 is "foobar". It is the concatenation of ["foo","bar"] which is a permutation of words.

Example 2:

Input: s = "wordgoodgoodgoodbestword", words = ["word","good","best","word"]

Output: []

Explanation:

There is no concatenated substring.

Example 3:

Input: s = "barfoofoobarthefoobarman", words = ["bar","foo","the"]

Output: [6,9,12]

Explanation:

The substring starting at 6 is "foobarthe". It is the concatenation of ["foo","bar","the"].
The substring starting at 9 is "barthefoo". It is the concatenation of ["bar","the","foo"].
The substring starting at 12 is "thefoobar". It is the concatenation of ["the","foo","bar"].

## Solution

```cpp
vector<int> findSubstring(string s, vector<string>& words) {
    auto siz = static_cast<int>(s.size());
    auto win = words[0].size();
    auto con = static_cast<int>(words.size() * win);

    auto ans = vector<int>();
    auto table = map<string, int>();

    for(auto word : words) {
        if(table.find(word) == table.end()) table[word] = 0;

        table[word] += 1;
    }

    for(auto off=0;off<win;off++) {
        auto left = off;
        auto matched = 0;
        auto cont = unordered_map<string, int>();

        for(auto right=off;right<siz;right += win) {
            auto tmp = s.substr(right, win);

            if(table.find(tmp) != table.end()) {
                matched++;

                if(cont.find(tmp) == cont.end()) cont[tmp] = 0;
                cont[tmp]++;

                while(cont[tmp] > table[tmp]) {
                    auto rm = s.substr(left, win);
                    matched--;
                    cont[rm]--;
                    left += win;
                }

                if(matched == words.size()) {
                    auto rm = s.substr(left, win);
                    
                    cout<<rm<<" "<<tmp<<endl;
                    cont[rm]--;
                    matched--;
                    ans.push_back(left);
                    left += win;   
                }
            }
            else {
                cont.clear();
                matched = 0;
                left = right + win;
            }
        }
    }

    return ans;
}
```

# 33. Search in Rotated Sorted Array

There is an integer array nums sorted in ascending order (with distinct values).

Prior to being passed to your function, nums is possibly left rotated at an unknown index k (1 <= k < nums.length) such that the resulting array is [nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]] (0-indexed). For example, [0,1,2,4,5,6,7] might be left rotated by 3 indices and become [4,5,6,7,0,1,2].

Given the array nums after the possible rotation and an integer target, return the index of target if it is in nums, or -1 if it is not in nums.

You must write an algorithm with O(log n) runtime complexity.

Example 1:

Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4

Example 2:

Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1

Example 3:

Input: nums = [1], target = 0
Output: -1

## Solution

```cpp
int search(vector<int>& nums, int target) {
    auto left = 0;
    auto right = static_cast<int>(nums.size() - 1);

    while(left<=right) {
        auto mid = (left + right) / 2;

        if(nums[mid] == target) return mid;

        auto on_left = (nums[left] <= target && target < nums[mid]);
        auto on_right = (nums[mid] < target && target <= nums[right]);

        if(on_left) {
            right = mid - 1;
        }
        else if(on_right) {
            left = mid + 1;
        }
        else if(nums[left] <= nums[mid]) {
            left = mid + 1;
        }
        else {
            right = mid - 1;
        }
    }

    return -1;
}
```

# 34. Find First and Last Position of Element in Sorted Array

Given an array of integers nums sorted in non-decreasing order, find the starting and ending position of a given target value.

If target is not found in the array, return [-1, -1].

You must write an algorithm with O(log n) runtime complexity.

## Solution

```cpp

```