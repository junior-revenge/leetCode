
# 128. Longest Consecutive Sequence

## Solution
```python
class Solution(object):
    def longestConsecutive(self, nums):
        num_set = set(nums)
        longest = 0

        for num in num_set:
            if num - 1 not in num_set:
                current = num
                streak = 1

                while current + 1 in num_set:
                    current += 1
                    streak += 1

                if streak > longest:
                    longest = streak

        return longest
```

## Key Takeway
brute force로 한다면 순회하면서 각 숫자에서 시작해서
1 더 큰 숫자를 찾다가 더 못 찾으면 거기서 길이를 기록하고
다음 숫자로 넘어간다.

그러면 기본적으로 O(N^3)가 나올 것. 
```python
class Solution:
    def longestConsecutive(self, nums):
        longest_streak = 0

        for num in nums:
            current_num = num
            current_streak = 1

            while current_num + 1 in nums:
                current_num += 1
                current_streak += 1

            longest_streak = max(longest_streak, current_streak)

        return longest_streak
```
이런식이면 while문 조건 확인에서도 O(N)이 소모되고 또 while문 자체가 O(N)을 소모해서 반복되고
또 for loop이 전체 숫자를 대상으로 가니 거기서도 O(N)이 나온다.

당연히 정렬을 돌리고 하면 O(NlogN)까지 줄일 수 있다. 그러나 O(N)으로 줄이려면 해시셋을 써야 한다.
맵이 아닌 해시셋인 이유는 어차피 우리는 증가하는 숫자를 하나씩만 확인하면 되기 때문.

그리고 브루트 포스 과정 중에서 이 해시셋으로 탐색을 줄일 수 있는 부분을 찾아본다.
일단 현재 어떤 값이 있고 그보다 1 큰 값을 다시 배열에서 찾을때, 이때 배열이 아닌 해시셋에서 탐색하면
시간복잡도의 차수를 하나 줄일 수 있다.
```python
num_set = set(nums)
for num in num_set:
    current = num
    streak = 1
    while current + 1 in num_set:
        current += 1
        streak += 1
```	
배열을 단순히 set()함수를 이용해 해쉬셋에 담는 것은 O(N)이 소모된다.
위와 같이 하면 이제 while문의 조건 부분이 O(1)에 탐색이 되니 전체 시간복잡도가 O(N^2)정도로 줄어든다.

그럼 여기서 더 최적화를 하기 위해서는 중복되는 탐색을 줄여야 한다. 예를 들어,
nums = [100,4,200,1,3,2]
이렇게 되어있으면, 1에서 시작해서 1->2->3->4 이렇게 탐색을 한번 했으면 그 다음에 2부터 시작하는 탐색
3부터 시작하는 탐색, 4부터 시작하는 탐색은 전부 할 필요가 없을 것이다.

여기서 약간 재간을 부려야하는데, 어떤 숫자가 저러한 탐색의 시작점인지, 아니면 어떤 다른 숫자에서 시작해서 하나씩
숫자를 찾아가는 과정의 중간에 위치한 숫자인지를 쉽게 알아내야 하는데, 그러기 위해서 탐색을 진행하려는 어떤 숫자에서
1을 뺀 값이 해쉬셋에 존재하는지 확인하고 없으면 탐색을 진행하고 있으면 안하는 것이다. 
해쉬셋을 어차피 기존 배열에서 어떤 숫자든 찾아내는 것은 빠르게 만들어놨으니 가능한 방법.



# 228. Summary Ranges

## Solution
```python

```

## Key Takeway





# 56. Merge Intervals

## Solution
```python

```

## Key Takeway
