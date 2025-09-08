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