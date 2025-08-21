# Longest Palindromic Substring

## Description

Given a string s, return the longest in s.

Example 1:

Input: s = "babad"
Output: "bab"
Explanation: "aba" is also a valid answer.

Example 2:

Input: s = "cbbd"
Output: "bb"

## Solution

We can solve this problem with DP and Two Pointer approach. We can still solve this without DP, but it would be slower. So it's a trade-off between time-complexity and space-complexity.

```cpp
string longestPalindrome(string s) {
    auto siz = s.size();
    auto dp = vector<vector<bool>>(siz, vector<bool>(siz, false));
    
    auto srt = 0;
    auto lon = 1;

    for(size_t i=0;i<siz;i++) dp[i][i] = true;
    
    for(size_t i=0;i<siz-1;i++) {
        if(s[i] == s[i+1]) {
            dp[i][i+1] = true;

            lon = 2;
            srt = i;
        }
    }

    for(size_t i=3;i<=siz;i++) {
        auto len = i - 1;

        for(size_t k=0;k<siz-len;k++) {
            if(s[k] == s[k+len] && dp[k+1][k+len-1]) {
                dp[k][k+len] = true;
                lon = i;
                srt = k;
            }
        }
    }

    return s.substr(srt, lon);
}
```