# 44. Wildcard Matching

Given an input string (s) and a pattern (p), implement wildcard pattern matching with support for '?' and '*' where:

- '?' Matches any single character.
- '*' Matches any sequence of characters (including the empty sequence).

The matching should cover the entire input string (not partial).

## Solution

```cpp
bool isMatch(string s, string p) {
    auto o = s.size(), t = p.size();
    auto dp = vector<vector<bool>>(o + 1, vector<bool>(t + 1, false));

    dp[0][0] = true;

    for(auto i=1;i<=t;i++) {
        if(p[i-1]=='*') dp[0][i] = dp[0][i-1];
    }

    for(auto i=0;i<o;i++) {
        for(auto j=0;j<t;j++) {
            auto c = p[j];

            if(c == '*') {
                dp[i+1][j+1] = dp[i+1][j] || dp[i][j+1];
            }
            else if(c == '?' || c == s[i]) {
                dp[i+1][j+1] = dp[i][j];
            }
        }
    }

    return dp[o][t];
}
```

# 47. Permutations II

Given a collection of numbers, nums, that might contain duplicates, return all possible unique permutations in any order.

## Solution

```cpp
class Solution {
public:
    set<vector<int>> chk;
    vector<vector<int>> ans;

    vector<vector<int>> permuteUnique(vector<int>& nums) {
        auto siz = nums.size();
        auto box = vector<int>();
        auto cand = vector<int>();

        for(int i=0;i<siz;i++) {
            cand.push_back(i);
        }
        
        track(nums, box, cand);

        return ans;
    }

    void track(vector<int>& nums, vector<int>& box, vector<int> cand) {
        if(cand.empty()) {
            if(chk.contains(box) == false){
                ans.push_back(box);
                chk.insert(box);
            }
            
            return;
        }

        for(auto i=0;i<cand.size();i++) {
            box.push_back(nums[cand[i]]);
            auto cpy = cand;
            cpy.erase(cpy.begin() + i);
            track(nums, box, cpy);
            box.pop_back();
        }
    }
};
```

# 48. Rotate Image

You are given an n x n 2D matrix representing an image, rotate the image by 90 degrees (clockwise).

You have to rotate the image in-place, which means you have to modify the input 2D matrix directly. DO NOT allocate another 2D matrix and do the rotation.

## Solution

```cpp
void rotate(vector<vector<int>>& matrix) {
    auto N = matrix.size();

    for(auto i=0;i<N;i++) {
        for(auto j=i+1;j<N;j++) {
            swap(matrix[i][j], matrix[j][i]);
        }
    }

    for(auto i=0; i<N;i++) {
        for(auto j=0;j<N/2;j++) {
            auto op = N - j;

            swap(matrix[i][j], matrix[i][op-1]);
        }
    }
}
```