# 52. N-Queens II

The n-queens puzzle is the problem of placing n queens on an n x n chessboard such that no two queens attack each other.

Given an integer n, return the number of distinct solutions to the n-queens puzzle.

## Solution

```cpp
class Solution {
public:
    int ans = 0;

    vector<bool> col;
    vector<bool> diag1;
    vector<bool> diag2;

    int totalNQueens(int n) {
        col.resize(n, false);
        diag1.resize(n * 2 - 1, false);
        diag2.resize(n * 2 - 1, false);

        track(0, n);

        return ans;    
    }

    void track(int r, int n) {
        if(r == n) {
            ans++;
            return;
        }

        for(auto c=0;c<n;c++) {
            if(col[c] || diag1[c - r + n - 1] || diag2[c + r]) {
                continue;
            }

            diag1[c - r + n - 1] = true;
            diag2[c + r] = true;
            col[c] = true;
            track(r + 1, n);
            diag1[c - r + n - 1] = false;
            diag2[c + r] = false;
            col[c] = false;
        }
    }
};
```

# 56. Merge Intervals

Given an array of intervals where intervals[i] = [starti, endi], merge all overlapping intervals, and return an array of the non-overlapping intervals that cover all the intervals in the input.

## Solution

```cpp
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        auto siz = intervals.size();
        auto ans = vector<vector<int>>();
        ans.push_back(intervals[0]);

        for(auto i=1;i<siz;i++) {
            auto one = ans.back();
            auto two = intervals[i];

            if(one[1] >= two[0]) {
                auto mer = merge(one, two);
                ans.back() = mer;
            }
            else {
                ans.push_back(two);
            }
        }

        return ans;
    }

    vector<int> merge(vector<int> o, vector<int> t) {
        o.insert( o.end(), t.begin(), t.end() );
    
        sort(o.begin(), o.end());

        return vector<int>{ o.front(), o.back() };
    }
};
```

# 54. Spiral Matrix

Given an m x n matrix, return all elements of the matrix in spiral order.

## Solution

```cpp
class Solution {
public:
    enum Direction {
        UP,
        DOWN,
        LEFT,
        RIGHT
    };

    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        auto m = matrix.size();
        auto n = matrix[0].size();

        auto visit = vector<vector<bool>>(m , vector<bool>(n, false));

        auto ans = vector<int>();
        auto dir = Direction::RIGHT;

        auto r = 0;
        auto c = 0;

        while(ans.size() < m * n) {
            switch(dir) {
                case Direction::UP:
                    for(auto i=r;i>=0;i--) {
                        if(visit[i][c]) {
                            c = c + 1;
                            dir = Direction::RIGHT;
                            break;
                        }

                        r = i;
                        visit[i][c] = true;
                        ans.push_back(matrix[i][c]);
                    }
                    break;
                case Direction::DOWN:
                    for(auto i=r;i<m;i++) {
                        if(visit[i][c]) {
                            c = c - 1;
                            dir = Direction::LEFT;
                            break;
                        }

                        r = i;
                        visit[i][c] = true;
                        ans.push_back(matrix[i][c]);
                    }
                    break;
                case Direction::LEFT:
                    for(auto i=c;i>=0;i--) {
                        if(visit[r][i]) {
                            r = r - 1;
                            dir = Direction::UP;
                            break;
                        }

                        c = i;
                        visit[r][i] = true;
                        ans.push_back(matrix[r][i]);
                    }
                    break;
                case Direction::RIGHT:
                    for(auto i=c;i<n;i++) {
                        if(visit[r][i]) {
                            r = r + 1;
                            dir = Direction::DOWN;
                            break;
                        }
                        c = i;
                        visit[r][i] = true;
                        ans.push_back(matrix[r][i]);
                    }
                    break;
            }
        }

        return ans;
    }
};
```