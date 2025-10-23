# 41. First Missing Positive

Given an unsorted integer array nums. Return the smallest positive integer that is not present in nums.

You must implement an algorithm that runs in O(n) time and uses O(1) auxiliary space.

## Solution

```cpp
int firstMissingPositive(vector<int>& nums) {
    auto siz = nums.size();

    for(int i=0;i<siz;i++) {
        while(0 < nums[i] && nums[i] <= siz && nums[nums[i] - 1] != nums[i]) {
            swap(nums[i], nums[nums[i]-1]);
        }
    }

    for(int i=0;i<siz;i++) {
        if(nums[i] != i + 1) return i + 1;
    }

    return siz + 1;
}
```

# 43. Multiply Strings

Given two non-negative integers num1 and num2 represented as strings, return the product of num1 and num2, also represented as a string.

Note: You must not use any built-in BigInteger library or convert the inputs to integer directly.

## Solution

```cpp
string multiply(string num1, string num2) {
    if(num1 == "0" || num2 == "0") return "0";

    auto t = num2.size(), o = num1.size();
    auto ans = vector<int>(o + t, 0);

    for(int i=o-1; i>=0;i--) {
        for(int j=t-1; j>=0;j--) {
            auto mul = (num1[i] - '0') * (num2[j] - '0');
            int p1 = i+j, p2 = p1+1;
            auto sum = mul + ans[p2];

            ans[p2] = sum % 10;
            ans[p1] += sum / 10;
        }
    }

    auto ret = string();

    for(auto n : ans) {
        if(ret.empty() && n == 0) continue;
        ret += to_string(n);
    }

    return ret;
}
```