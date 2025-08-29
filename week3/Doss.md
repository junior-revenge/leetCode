# Regular Expression Matching

Given an input string s and a pattern p, implement regular expression matching with support for '.' and '*' where:

    '.' Matches any single character.​​​​
    '*' Matches zero or more of the preceding element.

The matching should cover the entire input string (not partial).

Example 1:

Input: s = "aa", p = "a"
Output: false
Explanation: "a" does not match the entire string "aa".

Example 2:

Input: s = "aa", p = "a*"
Output: true
Explanation: '*' means zero or more of the preceding element, 'a'. Therefore, by repeating 'a' once, it becomes "aa".

Example 3:

Input: s = "ab", p = ".*"
Output: true
Explanation: ".*" means "zero or more (*) of any character (.)".

## Solution

```cpp
class Solution {
public:
    bool isMatch(string s, string p) {
        auto M = s.size();
        auto N = p.size();

        auto dp = vector<vector<bool>>(M + 1, vector<bool>(N+1));
        dp[0][0] = true;

        for (auto n = 2; n <= N; n+=2) {
            if (p[n - 1] == '*') {
                dp[0][n] = dp[0][n - 2];
            }
        }

        for(size_t m=1;m<=M;m++) {
            for(size_t n=1;n<=N;n++) {

                if(p[n-1] == '*') {
                    dp[m][n] = dp[m][n-2];

                    if(s[m-1]==p[n-2] || p[n-2]=='.') {
                        dp[m][n] = dp[m][n] || dp[m-1][n];
                    }
                }
                else {
                    if(s[m-1]==p[n-1] || p[n-1] == '.') {
                        dp[m][n] = dp[m-1][n-1];
                    }
                }
            }
        }

        return dp[M][N];
    }
};
```

# 16. 3Sum Closest

Given an integer array nums of length n and an integer target, find three integers in nums such that the sum is closest to target.

Return the sum of the three integers.

You may assume that each input would have exactly one solution.

Example 1:

Input: nums = [-1,2,1,-4], target = 1
Output: 2
Explanation: The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).

Example 2:

Input: nums = [0,0,0], target = 1
Output: 0
Explanation: The sum that is closest to the target is 0. (0 + 0 + 0 = 0).

## Solution

```cpp
int threeSumClosest(vector<int>& nums, int target) {
    sort(nums.begin(), nums.end());

    auto ans = 0;
    auto dif = INT_MAX;
    auto siz = nums.size();

    for(size_t piv=0; piv<siz-2;piv++) {
        auto start = piv+1;
        auto end = siz-1;

        while(start<end) {
            auto sum = nums[piv] + nums[start] + nums[end];
            auto gap = abs(target - sum);

            if(gap < dif) {
                dif = gap;
                ans = sum;
            }

            if(sum == target) return sum;
            else if(sum > target) {
                end--;
            }
            else if(sum < target) {
                start++;
            }
        }
    }

    return ans;
}
```

# Letter Combinations of a Phone Number

Given a string containing digits from 2-9 inclusive, return all possible letter combinations that the number could represent. Return the answer in any order.

A mapping of digits to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

Example 1:

Input: digits = "23"
Output: ["ad","ae","af","bd","be","bf","cd","ce","cf"]

Example 2:

Input: digits = ""
Output: []

Example 3:

Input: digits = "2"
Output: ["a","b","c"]

## Solution

```cpp
vector<string> letterCombinations(string digits) {
    auto m = map<char, string> {
        { '2', "abc" }, { '3', "def" },
        { '4', "ghi" }, { '5', "jkl" },
        { '6', "mno" }, { '7', "pqrs"},
        { '8', "tuv" }, { '9', "wxyz"}
    };

    auto ans = vector<string>();
    if(digits.empty()) return ans;

    ans.push_back("");

    for(auto digit : digits) {
        auto str = m[digit];
        auto temp = vector<string>();

        for(auto& comb : ans) {
            for(auto c : str) {
                temp.push_back(comb + c);   
            }
        }

        ans = temp;
    }

    return ans;
}
```