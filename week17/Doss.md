# 64. Minimum Path Sum

Given a m x n grid filled with non-negative numbers, find a path from top left to bottom right, which minimizes the sum of all numbers along its path.

Note: You can only move either down or right at any point in time.

## Solution

```cpp
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        const auto M = grid.size();
        const auto N = grid[0].size();
        auto dp = vector<vector<int>>(M, vector<int>(N, 0));

        for(auto i=0;i<M;i++) {
            for(auto j=0;j<N;j++) {
                dp[i][j] = grid[i][j];

                if(i == 0 && j == 0) continue;

                auto u = INT_MAX, l = INT_MAX;

                if(i != 0) u = dp[i-1][j];
                if(j != 0) l = dp[i][j-1];
            
                dp[i][j] += u > l ? l : u;
            }
        }

        return dp.back().back();
    }
};
```

# 65. Valid Number

Given a string s, return whether s is a valid number.

For example, all the following are valid numbers: "2", "0089", "-0.1", "+3.14", "4.", "-.9", "2e10", "-90E3", "3e+7", "+6e-1", "53.5e93", "-123.456e789", while the following are not valid numbers: "abc", "1a", "1e", "e3", "99e2.5", "--6", "-+3", "95a54e53".

Formally, a valid number is defined using one of the following definitions:

    An integer number followed by an optional exponent.
    A decimal number followed by an optional exponent.

An integer number is defined with an optional sign '-' or '+' followed by digits.

A decimal number is defined with an optional sign '-' or '+' followed by one of the following definitions:

    Digits followed by a dot '.'.
    Digits followed by a dot '.' followed by digits.
    A dot '.' followed by digits.

An exponent is defined with an exponent notation 'e' or 'E' followed by an integer number.

The digits are defined as one or more digits.

## Solution

```cpp
class Solution {
public:
    bool isNumber(string s) {
        bool digit = false;
        bool dot = false;
        bool exponent = false;

        for (int i = 0; i < s.length(); i++) {
            char c = s[i];

            if (c <= '9' && '0' <= c) {
                digit = true;
            }
            else if (c == '+' || c == '-') {
                if (i > 0 && s[i - 1] != 'e' && s[i - 1] != 'E') {
                    return false;
                }
            }
            else if (c == 'e' || c == 'E') {
                if (exponent || !digit) {
                    return false;
                }
                exponent = true;
                digit = false;
            }
            else if (c == '.') {
                if (dot || exponent) {
                    return false;
                }
                dot = true;
            }
            else {
                return false;
            }
        }

        return digit;
    }
};
```

# 66. Plus One

You are given a large integer represented as an integer array digits, where each digits[i] is the ith digit of the integer. The digits are ordered from most significant to least significant in left-to-right order. The large integer does not contain any leading 0's.

Increment the large integer by one and return the resulting array of digits.

## Solution

```cpp
class Solution {
public:
    vector<int> plusOne(vector<int>& digits) {
        auto siz = static_cast<int>(digits.size());

        for(auto i=siz-1;i>=0;i--) {
            if(digits[i] == 9) {
                digits[i] = 0;
                
                if(i == 0) {
                    digits.insert(digits.begin(), 1, 1);
                    break;
                }
                
                continue;
            }

            digits[i]++;
            break;
        }

        return digits;
    }
};
```