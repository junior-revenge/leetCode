# 39. Combination Sum

Given an array of distinct integers candidates and a target integer target, return a list of all unique combinations of candidates where the chosen numbers sum to target. You may return the combinations in any order.

The same number may be chosen from candidates an unlimited number of times. Two combinations are unique if the

of at least one of the chosen numbers is different.

The test cases are generated such that the number of unique combinations that sum up to target is less than 150 combinations for the given input.

## Solution

```cpp
vector<vector<int>> ans;

vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
    vector<int> box;

    backtrack(candidates, target, 0, box, ans, 0);

    return ans;
}

void backtrack(vector<int>& candidates, int target, int start, 
                vector<int>& current, vector<vector<int>>& result, int currentSum) {
    if (currentSum == target) {
        result.push_back(current);
        return;
    }
    
    if (currentSum > target) {
        return;
    }
    
    for (int i = start; i < candidates.size(); i++) {
        current.push_back(candidates[i]);
        backtrack(candidates, target, i, current, result, currentSum + candidates[i]);
        current.pop_back();
    }
}
```

# 40. Combination Sum II

Given a collection of candidate numbers (candidates) and a target number (target), find all unique combinations in candidates where the candidate numbers sum to target.

Each number in candidates may only be used once in the combination.

Note: The solution set must not contain duplicate combinations.

## Solution

```cpp
vector<vector<int>> ans;

vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
    auto box = vector<int>();
    sort(candidates.begin(), candidates.end());

    track(box, 0, 0, candidates, target);
    
    return ans;
}

void track(vector<int>& box, int sum, int idx, vector<int>& cand, int target) {
    if(sum > target) return;
    if(sum == target) {
        ans.push_back(box);
        return;
    }

    for(auto i=idx;i<cand.size();i++) {
        auto c = cand[i];

        if(i > idx && cand[i] == cand[i-1]) continue; //remove duplication

        box.push_back(c);
        track(box, sum + c, i + 1, cand, target);
        box.pop_back();
    }
}
```

# 42. Trapping Rain Water

Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it can trap after raining.

## Solution

```cpp
int trap(vector<int>& height) {
    auto pour = 0;
    auto siz = height.size();

    auto simul = 0;
    auto high = 0;
    auto peak = 0;

    //left to right
    for(auto i=0;i<siz;i++) {
        auto h = height[i];
    
        if(h >= high) {
            pour += simul;
            simul = 0;
            high = h;

            peak = i;

            continue;
        }

        simul += high - h;
    }

    high = 0;
    simul = 0;

    //right to left
    for(auto k=siz;k>peak;k--) {
        auto i = k - 1;
        auto h = height[i];
    
        if(h >= high) {
            pour += simul;
            simul = 0;
            high = h;

            continue;
        }

        simul += high - h;
    }

    return pour;
}
```