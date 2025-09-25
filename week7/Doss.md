# 30. Substring with Concatenation of All Words

You are given a string s and an array of strings words. All the strings of words are of the same length.

A concatenated string is a string that exactly contains all the strings of any permutation of words concatenated.

    For example, if words = ["ab","cd","ef"], then "abcdef", "abefcd", "cdabef", "cdefab", "efabcd", and "efcdab" are all concatenated strings. "acdbef" is not a concatenated string because it is not the concatenation of any permutation of words.

Return an array of the starting indices of all the concatenated substrings in s. You can return the answer in any order.

Example 1:

Input: s = "barfoothefoobarman", words = ["foo","bar"]

Output: [0,9]

Explanation:

The substring starting at 0 is "barfoo". It is the concatenation of ["bar","foo"] which is a permutation of words.
The substring starting at 9 is "foobar". It is the concatenation of ["foo","bar"] which is a permutation of words.

Example 2:

Input: s = "wordgoodgoodgoodbestword", words = ["word","good","best","word"]

Output: []

Explanation:

There is no concatenated substring.

Example 3:

Input: s = "barfoofoobarthefoobarman", words = ["bar","foo","the"]

Output: [6,9,12]

Explanation:

The substring starting at 6 is "foobarthe". It is the concatenation of ["foo","bar","the"].
The substring starting at 9 is "barthefoo". It is the concatenation of ["bar","the","foo"].
The substring starting at 12 is "thefoobar". It is the concatenation of ["the","foo","bar"].

## Solution

This problem asks us to check if a given string contains combinations of strings from the words array and return the starting indices of substrings that match. Before solving this problem, there are key points to consider: all strings in the words array have the same length, and these strings must be concatenated consecutively.

This leads us to the sliding window approach. We can iterate through a certain range and check if each substring is a valid concatenation of the words. However, several challenges arise.

First, simply iterating from 1 to N won't be sufficient. How can we ensure we cover all possible cases? Second, how should we handle overlapping concatenated strings in string s? The solution is simpler than it seems.

We need to iterate through the range 1 to N, but set our starting points within the length of each word in words. If we denote the word length as k, we perform k * N iterations. This works because when we slide a window of length k, any consecutive combination of k-length strings will have a starting point within the first k positions.

We also need a dynamic programming-like approach to efficiently check consecutive concatenated strings. For example, suppose words = ['wr', 'rw', 'ad'] and string s = 'wrrwadwrrw'. In this case, we should return [0, 2, 4]. To optimize this, we need to memoize previous results.

Therefore, we use a hash map (dictionary) to track the count of consecutive words from words that appear in the current window. We check the validity at each step, and when we find a match, we slide the window by removing the leftmost k-length substring and continue checking. This achieves optimization while covering all possible cases.

Let N be the length of string s, J be the length of the words array, and K be the length of each word in words.
The final time complexity is O(K * N), and the space complexity is O(K * J).

```cpp
vector<int> findSubstring(string s, vector<string>& words) {
    auto siz = static_cast<int>(s.size());
    auto win = words[0].size();
    auto con = static_cast<int>(words.size() * win);

    auto ans = vector<int>();
    auto table = map<string, int>();

    for(auto word : words) {
        if(table.find(word) == table.end()) table[word] = 0;

        table[word] += 1;
    }

    for(auto off=0;off<win;off++) {
        auto left = off;
        auto matched = 0;
        auto cont = unordered_map<string, int>();

        for(auto right=off;right<siz;right += win) {
            auto tmp = s.substr(right, win);

            if(table.find(tmp) != table.end()) {
                matched++;

                if(cont.find(tmp) == cont.end()) cont[tmp] = 0;
                cont[tmp]++;

                while(cont[tmp] > table[tmp]) {
                    auto rm = s.substr(left, win);
                    matched--;
                    cont[rm]--;
                    left += win;
                }

                if(matched == words.size()) {
                    auto rm = s.substr(left, win);
                    
                    cout<<rm<<" "<<tmp<<endl;
                    cont[rm]--;
                    matched--;
                    ans.push_back(left);
                    left += win;   
                }
            }
            else {
                cont.clear();
                matched = 0;
                left = right + win;
            }
        }
    }

    return ans;
}
```

# 33. Search in Rotated Sorted Array

There is an integer array nums sorted in ascending order (with distinct values).

Prior to being passed to your function, nums is possibly left rotated at an unknown index k (1 <= k < nums.length) such that the resulting array is [nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]] (0-indexed). For example, [0,1,2,4,5,6,7] might be left rotated by 3 indices and become [4,5,6,7,0,1,2].

Given the array nums after the possible rotation and an integer target, return the index of target if it is in nums, or -1 if it is not in nums.

You must write an algorithm with O(log n) runtime complexity.

Example 1:

Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4

Example 2:

Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1

Example 3:

Input: nums = [1], target = 0
Output: -1

## Solution

### Korean

주어진 조건들을 보았을 때에 이진 탐색을 어렵지 않게 떠올릴 수가 있다. 하지만 단순히 mid index를 비교하여 좌우 이동을 하는 것만으로는 충분하지 못하다. 여기서 고려 할 부분은 오름차순으로 정렬 된 배열을 회전 이동을 했다는 것이다. 여기서 절반은 무조건 오름차순이라는 것이 보장되며, 나머지는 그렇지 못함을 알 수 있다.

그렇다면 어느쪽이 오름차순으로 정렬 되었음을 어떻게 알 수가 있을까? 답은 간단하다. 그냥 첫 값과 마지막 값과 mid index와 비교하면 된다. 그래서 마지막 인덱스 값이 mid 인덱스 보다 크다면 오른쪽이 정렬 되었음을 알 수가 있다. 그 역도 마찬가지다. 그래서 우리가 찾는 값이 정렬 된 배열 속에 있다면 기존의 이진 탐색으로 해결 할 수가 있다.

그렇지만 정렬되지 않은 곳이라면? 이 경우엔 명백히 정렬 된 배열 내에 없음을 확인한 후에 탐색을 한다. 그리고 이걸 똑같이 반복한다. 왜냐하면 정렬 된 배열 범위내에서 명백히 존재하지 않음을 확인했다면, 우리는 그런 탐색을 시도 할 필요가 없다. 하지만 나머지에 대해서는 명확하지 않고, 일말의 가능성이 남아있기에 탐색을 시도해보는 것이다. 

그래서 만약에 존재한다면 찾을 수가 있을 것이고, 없다면 -1 을 반환할 것이다. 이렇게 이런 접근법을 통해 우리는 회전 이동한 배열에서 이진 탐색을 적용 할 수가 있다.

### English

Looking at the given constraints, we can easily think of binary search. However, simply comparing the mid index and moving left or right isn't sufficient. The key consideration here is that we have an ascending sorted array that has been rotated. We know that one half is guaranteed to be monotonically increasing, while the other half is not.

So how can we determine which side is sorted in ascending order? The answer is simple. We just need to compare the first value, last value, and mid index. If the value at the last index is greater than the value at the mid index, we know the right half is sorted. The same logic applies in reverse. Therefore, if the target value we're looking for is within the sorted portion, we can solve it using conventional binary search.

But what if it's in the unsorted portion? In this case, we first confirm that the target clearly doesn't exist within the sorted array, then we search the other half. We repeat this process because if we've clearly confirmed that the target doesn't exist within the sorted range, there's no need to attempt that search. However, for the remaining portion, it's unclear, but there's still a possibility, so we attempt the search there.

Through this approach, if the target exists, we'll be able to find it; if not, we'll return -1. This way, we can apply binary search to a rotated sorted array.

```cpp
int search(vector<int>& nums, int target) {
    auto left = 0;
    auto right = static_cast<int>(nums.size() - 1);

    while(left<=right) {
        auto mid = (left + right) / 2;

        if(nums[mid] == target) return mid;

        auto on_left = (nums[left] <= target && target < nums[mid]);
        auto on_right = (nums[mid] < target && target <= nums[right]);

        if(on_left) {
            right = mid - 1;
        }
        else if(on_right) {
            left = mid + 1;
        }
        else if(nums[left] <= nums[mid]) {
            left = mid + 1;
        }
        else {
            right = mid - 1;
        }
    }

    return -1;
}
```

# 34. Find First and Last Position of Element in Sorted Array

Given an array of integers nums sorted in non-decreasing order, find the starting and ending position of a given target value.

If target is not found in the array, return [-1, -1].

You must write an algorithm with O(log n) runtime complexity.

Example 1:

Input: nums = [5,7,7,8,8,10], target = 8
Output: [3,4]

Example 2:

Input: nums = [5,7,7,8,8,10], target = 6
Output: [-1,-1]

Example 3:

Input: nums = [], target = 0
Output: [-1,-1]

## Solution

### Korean

 주어진 조건들을 보았을 때에 이진 탐색을 이용하여 풀 수 있음을 어렵지않게 떠올 수 있다. 하지만 단순하게 기존의 이진 탐색을 적용 하는 것으로는 부족하다. 어떻게 푸는 것이 좋을까?

 기본적으로 이진 탐색의 접근법은 존재 할 것으로 기대되는 범위를 반씩 줄여가며 탐색하는 것이 주된 골자다. 그렇기에 어떻게 반씩 좁혀가며 탐색을 할 수가 있을까. 우리가 찾는 target은 2개다. 어떠한 숫자의 시작 및 끝을 찾는 것인데(가령 중복 된 갯수로 존재 할 경우), 이를 충족하는 조건은 다음과 같다.

 시작점: 인덱스 i 에 대해, i 번쨰 값이 i - 1 번쨰 값 보다 크가.
 끝점: 인덱스 i에 대해, i 번째 값이 i + 1 보다 작다.

 이러한 점을 고려 해 보았을 때에 각각 arr[i] > arr[i-1], arr[i] < arr[i+1] 을 탐색하는 문제로 접근 할 수가 있다. 그리고 탐색 조건을 해당 숫자가 인근한 배열과 같이 같은지 다른지를 체크하는 것으로 확인이 가능하다.

 그래서 시간 복잡도는 2 * log N 이 되며, 공간 복잡도는 constant value가 된다.

### English

Looking at the given constraints, we can easily think of using binary search to solve this problem. However, simply applying conventional binary search isn't sufficient. How should we approach this?

The fundamental approach of binary search is to reduce the search space by half while searching within the expected range. So how can we narrow down the search by half? We're looking for two targets: the starting and ending positions of a given number (when it exists as duplicates). The conditions that satisfy this are as follows:

Starting point: For index i, the value at index i is greater than the value at index i-1.
Ending point: For index i, the value at index i is less than the value at index i+1.

Considering these points, we can approach this as searching for arr[i] > arr[i-1] and arr[i] < arr[i+1] respectively. We can confirm the search condition by checking whether the target number is the same or different from its adjacent array elements.

Therefore, the time complexity is 2 * log N, and the space complexity is constant.

```cpp
vector<int> searchRange(vector<int>& nums, int target) {
    auto siz = static_cast<int>(nums.size());
    
    auto left = 0;
    auto right = siz - 1;

    auto ans = vector<int> { -1, -1 };

    while(left <= right) {
        auto mid = (left + right) / 2;

        if(nums[mid] == target) {
            if(mid == 0 || nums[mid-1] != target) {
                ans[0] = mid;
                break;
            }
            else {
                right = mid - 1;
                continue;   
            }
        }

        if(nums[mid] > target) {
            right = mid - 1;
        }
        else {
            left = mid + 1;
        }
    }

    left = 0;
    right = siz - 1;

    while(left <= right) {
        auto mid = (left + right) / 2;

        if(nums[mid] == target) {
            if(mid == siz - 1 || nums[mid + 1] != target) {
                ans[1] = mid;
                break;
            }
            else {
                left = mid + 1;
                continue;   
            }
        }

        if(nums[mid] > target) {
            right = mid - 1;
        }
        else {
            left = mid + 1;
        }
    }

    return ans;
}
```