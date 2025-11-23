# 57. Insert Interval

You are given an array of non-overlapping intervals intervals where intervals[i] = [starti, endi] represent the start and the end of the ith interval and intervals is sorted in ascending order by starti. You are also given an interval newInterval = [start, end] that represents the start and end of another interval.

Insert newInterval into intervals such that intervals is still sorted in ascending order by starti and intervals still does not have any overlapping intervals (merge overlapping intervals if necessary).

Return intervals after the insertion.

Note that you don't need to modify intervals in-place. You can make a new array and return it.

## Solution

```cpp
vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
    intervals.push_back(newInterval);
    sort(intervals.begin(), intervals.end());

    auto ans = vector<vector<int>>();

    ans.push_back(intervals[0]);

    for(auto i=1;i<intervals.size();i++) {
        auto pre = ans.back()[1];
        auto cur = intervals[i][0];

        if(pre >= cur) {
            ans.back()[1] = intervals[i][1] > pre ? intervals[i][1] : pre;
        }
        else {
            ans.push_back(intervals[i]);
        }
    }

    return ans;
}
```

# 59. Spiral Matrix II

Given a positive integer n, generate an n x n matrix filled with elements from 1 to n2 in spiral order.

## Solution

```cpp
vector<vector<int>> generateMatrix(int n) {
    vector<vector<int>> matrix(n, vector<int>(n, 0));
    
    int top = 0, bottom = n - 1;
    int left = 0, right = n - 1;
    int num = 1;
    
    while (num <= n * n) {
        for (int i = left; i <= right; i++)
            matrix[top][i] = num++;
        top++;
        
        for (int i = top; i <= bottom; i++)
            matrix[i][right] = num++;
        right--;
        
        for (int i = right; i >= left; i--)
            matrix[bottom][i] = num++;
        bottom--;
        
        for (int i = bottom; i >= top; i--)
            matrix[i][left] = num++;
        left++;
    }
    
    return matrix;
}
```

# 60. Permutation Sequence

The set [1, 2, 3, ..., n] contains a total of n! unique permutations.

By listing and labeling all of the permutations in order, we get the following sequence for n = 3:

    "123"
    "132"
    "213"
    "231"
    "312"
    "321"

Given n and k, return the kth permutation sequence.

## Solution

```cpp
string getPermutation(long long n, long long k) {
    auto fac = vector<long long>(n);
    fac[0] = 1;

    for(long long i = 1; i < n; i++) {
        fac[i] = fac[i-1] * i;
    }

    auto cand = vector<int>();
    for(int i = 1; i <= n; i++) {
        cand.push_back(i);
    }

    string ans = "";
    k--;  // 0-indexed로 변환
    
    for(long long i = n; i >= 1; i--) {
        long long div = fac[i-1];
        int idx = k / div;
        
        ans += to_string(cand[idx]);
        cand.erase(cand.begin() + idx);
        k = k % div;
    }

    return ans;
}
```