# 61. Rotate List

Given the head of a linked list, rotate the list to the right by k places.

## Solution

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* rotateRight(ListNode* head, int k) {
        if(head == nullptr) return head;
        
        auto v = vector<ListNode*>();
        auto cur = head;
    
        while(cur != nullptr) {
            v.push_back(cur);
            cur = cur->next;
        }

        v.back()->next = v.front();

        k = k % v.size();
        auto siz = v.size();

        v[siz - 1 - k]->next = nullptr;

        if(k == 0) return v.front();
        return v[siz - k];
    }
};
```

# 62. Unique Paths

There is a robot on an m x n grid. The robot is initially located at the top-left corner (i.e., grid[0][0]). The robot tries to move to the bottom-right corner (i.e., grid[m - 1][n - 1]). The robot can only move either down or right at any point in time.

Given the two integers m and n, return the number of possible unique paths that the robot can take to reach the bottom-right corner.

The test cases are generated so that the answer will be less than or equal to 2 * 109.

## Solution

```cpp
class Solution {
public:
    int uniquePaths(int m, int n) {
        auto dp = vector<vector<int>>(m, vector<int>(n, 0));

        dp[0][0] = 1;

        for(auto i=0;i<m;i++) {
            for(auto k=0;k<n;k++) {
                if(i != 0) { dp[i][k] += dp[i-1][k]; }
                if(k != 0) { dp[i][k] += dp[i][k-1]; }
            }
        }

        return dp.back().back();
    }
};
```

# 63. Unique Paths II

You are given an m x n integer array grid. There is a robot initially located at the top-left corner (i.e., grid[0][0]). The robot tries to move to the bottom-right corner (i.e., grid[m - 1][n - 1]). The robot can only move either down or right at any point in time.

An obstacle and space are marked as 1 or 0 respectively in grid. A path that the robot takes cannot include any square that is an obstacle.

Return the number of possible unique paths that the robot can take to reach the bottom-right corner.

The testcases are generated so that the answer will be less than or equal to 2 * 109.

## Solution

```cpp
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        auto N = obstacleGrid.size();
        auto M = obstacleGrid[0].size();

        auto dp = vector<vector<int>>(N, vector<int>(M, 0));

        for(auto i=0;i<N;i++) {
            for(auto j=0;j<M;j++) {
                if(obstacleGrid[i][j] == 1) continue;

                if(i == 0 && j == 0) {
                    dp[i][j] = 1;
                    continue;
                }
                
                if(i != 0) {
                    dp[i][j] += dp[i-1][j];
                }

                if(j != 0) {
                    dp[i][j] += dp[i][j-1];
                }
            }
        }

        return dp.back().back();
    }
};
```