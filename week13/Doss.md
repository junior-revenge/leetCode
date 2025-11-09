# 50. Pow(x, n)

Implement pow(x, n), which calculates x raised to the power n (i.e., xn).

## Solution

```cpp
class Solution {
public:
    double myPow(double x, long long n) {
        if(n < 0) {
            n *= -1;
            x = 1/x;
        }

        return fastPow(x, n);
    }

    double fastPow(double x, long long n) {
        if(n == 0) return 1.0;

        auto half = fastPow(x, n/2);

        if(n % 2 == 0) {
            return half * half;
        }
        else {
            return x * half * half;
        }
    }
};
```

# 51. N-Queens

The n-queens puzzle is the problem of placing n queens on an n x n chessboard such that no two queens attack each other.

Given an integer n, return all distinct solutions to the n-queens puzzle. You may return the answer in any order.

Each solution contains a distinct board configuration of the n-queens' placement, where 'Q' and '.' both indicate a queen and an empty space, respectively.

## Solution

```cpp
class Solution {
public:
    vector<vector<string>> result;
    vector<int> queens;
    vector<bool> cols;

    vector<bool> diag1;
    vector<bool> diag2; 

    vector<vector<string>> solveNQueens(int n) {
        result.clear();
        queens.resize(n);
        cols.resize(n, false);
        diag1.resize(2 * n - 1, false);
        diag2.resize(2 * n - 1, false);
        
        backtrack(0, n);
        return result;
    }

    void backtrack(int row, int n) {
        if (row == n) {
            result.push_back(generateBoard(n));
            return;
        }
        
        for (int col = 0; col < n; col++) {
            if (cols[col] || diag1[row - col + n - 1] || diag2[row + col]) {
                continue;
            }
            
            queens[row] = col;
            cols[col] = true;
            diag1[row - col + n - 1] = true;
            diag2[row + col] = true;
            
            backtrack(row + 1, n);
            
            cols[col] = false;
            diag1[row - col + n - 1] = false;
            diag2[row + col] = false;
        }
    }
    
    vector<string> generateBoard(int n) {
        vector<string> board;

        for (int i = 0; i < n; i++) {
            string row(n, '.');
            row[queens[i]] = 'Q';
            board.push_back(row);
        }

        return board;
    }
};
```

# 53. Maximum Subarray

Given an integer array nums, find the with the largest sum, and return its sum.

## Solution

```cpp
int maxSubArray(vector<int>& nums) {
    auto mx = -10001;
    auto sum = 0;

    for(auto n : nums) {
        sum += n;

        if(mx < sum) mx = sum;

        if(sum < 0) sum = 0;
    }

    return mx;
}
```