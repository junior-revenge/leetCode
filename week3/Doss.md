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