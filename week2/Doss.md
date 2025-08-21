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

# String to Integer (atoi)

## Description

Implement the myAtoi(string s) function, which converts a string to a 32-bit signed integer.

The algorithm for myAtoi(string s) is as follows:

    Whitespace: Ignore any leading whitespace (" ").
    Signedness: Determine the sign by checking if the next character is '-' or '+', assuming positivity if neither present.
    Conversion: Read the integer by skipping leading zeros until a non-digit character is encountered or the end of the string is reached. If no digits were read, then the result is 0.
    Rounding: If the integer is out of the 32-bit signed integer range [-231, 231 - 1], then round the integer to remain in the range. Specifically, integers less than -231 should be rounded to -231, and integers greater than 231 - 1 should be rounded to 231 - 1.

Return the integer as the final result.

 

Example 1:

Input: s = "42"

Output: 42

Explanation:

The underlined characters are what is read in and the caret is the current reader position.

Step 1: "42" (no characters read because there is no leading whitespace)
         ^
Step 2: "42" (no characters read because there is neither a '-' nor '+')
         ^
Step 3: "42" ("42" is read in)
           ^

Example 2:

Input: s = " -042"

Output: -42

Explanation:

Step 1: "   -042" (leading whitespace is read and ignored)
            ^
Step 2: "   -042" ('-' is read, so the result should be negative)
             ^
Step 3: "   -042" ("042" is read in, leading zeros ignored in the result)
               ^

Example 3:

Input: s = "1337c0d3"

Output: 1337

Explanation:

Step 1: "1337c0d3" (no characters read because there is no leading whitespace)
         ^
Step 2: "1337c0d3" (no characters read because there is neither a '-' nor '+')
         ^
Step 3: "1337c0d3" ("1337" is read in; reading stops because the next character is a non-digit)
             ^

Example 4:

Input: s = "0-1"

Output: 0

Explanation:

Step 1: "0-1" (no characters read because there is no leading whitespace)
         ^
Step 2: "0-1" (no characters read because there is neither a '-' nor '+')
         ^
Step 3: "0-1" ("0" is read in; reading stops because the next character is a non-digit)
          ^

Example 5:

Input: s = "words and 987"

Output: 0

Explanation:

Reading stops at the first non-digit character 'w'.

## Solution

Just simple a string and implmentation problem.

```cpp
int myAtoi(string s) {
    auto siz = s.size();
    auto sign = false;

    auto min = 1;
    long long MAX = 1ULL <<31;
    long long ans = 0;

    for(size_t i=0;i<siz;i++) {
        auto ch = s[i];

        if(ch == ' ') {
            if(sign) break;

            continue;
        }

        if(sign == false) {
            if(ch == '+') {
                sign = true;
                continue;
            }

            if(ch == '-') {
                sign = true;
                min *= -1;
                continue;
            }
        }

        if(ch < '0' || ch > '9') break;
    
        ans *= 10;
        ans += ch - '0';

        if(ans >= MAX) {
            ans = MAX;

            if(min == 1) ans -= 1;

            break;
        }

        sign = true;
    }

    return ans * min;
}
```
