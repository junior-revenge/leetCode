
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
class Solution:
    def summaryRanges(self, nums):
        ranges = []     
        i = 0 
        
        while i < len(nums): 
            start = nums[i]  
            while i + 1 < len(nums) and nums[i] + 1 == nums[i + 1]: 
                i += 1 
            
            if start != nums[i]: 
                ranges.append(str(start) + "->" + str(nums[i]))
            else: 
                ranges.append(str(nums[i]))
            
            i += 1

        return ranges
```

## Key Takeway

연속되는 정수의 범위 묶음으로 주어진 정수 배열을 표시하란 문제다.
표시는 범위를 포함한다. 
0->2는 0, 1, 2를 의미한다. 
0은 0 -> 0 즉 그냥 0 하나를 의미한다.
따라서 주어진 정수 중에 이웃한 정수가 그 차이가 1보다 크면 같은 범위 안에 포함될 수가 없다.
그러나 그 차이가 1이라면 그 둘은 항상 같은 범위 안에 들어가야 할 것이다.
nums배열을 순회하면서 일단 첫 숫자는 첫 범주의 시작점으로 저장을 해야할 것이다.
그리고 그 다음 숫자를 확인하면서 1만 큰지 아니면 더 큰지를 판가름해야한다.
1만 크면 이걸 당연히 추가해야하고 다음 숫자를 확인할 것이다. 만약 1보다 더 큰 차이로 숫자가크다면 
또는 nums의 끝에 도달한다면 우리는 이 범위는 다 끝낸 셈이므로 마무리를 해주어야 한다.
근데 이 때 이 값 nums[i]와 start가 같은지 확인한다. 같다면 지금 이 범위엔 숫자 하나뿐이라는 이야기. 
이때는 숫자 두개가 아니라 그 하나의 값만 넣어줘야하므로 따로 처리해준다. 그렇지 않다면 범위 안에 숫자가
둘 이상 있다는 것이니 범위 형태로 표현한다. 'start -> nums[i]'를 추가해줘야 할것.
한 범위에서의 작업이 끝나면 i를 1 늘려 다시 반복한다.

시간복잡도는 O(N), 공간복잡도는 O(1).



# 56. Merge Intervals

## Solution
```python

```

## Key Takeway
