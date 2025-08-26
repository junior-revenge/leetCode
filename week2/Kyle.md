
# 80. Remove Duplicates from Sorted Array II

## Solution
```python

class Solution(object):
    def removeDuplicates(self, nums):
        second_pointer = 2
        for i in range(2, len(nums)):
            if nums[i] != nums[second_pointer - 2]:
                nums[second_pointer] = nums[i]
                second_pointer += 1
    
        return second_pointer

```

## Key Takeway
기본적으로 두 가지:

1) second_pointer는 업데이트할 값의 위치 역할을 한다. 비교는 이 second_pointer에서부터 두 칸 왼쪽의 값과 하고, 업데이트는 nums[i]의 값을 이 위치에 업데이트 한다. 이로 인해 second_pointer와 i가 일치해 동일한 값을 제자리에 업데이트하는 경우가 생기더라도 로직을 유지한다.

2) second_pointer를 언제 업데이트해야하는지 정확히 이해한다. nums[i]의 값이 nums[second_pointer - 2]의 값과 일치한다면 second_pointer가 움직일 이유가 없으며 둘의 값이 일치하지 않을 경우 second_pointer는 우측으로 한 칸만 이동하는 것으로 충분하다는 것을 정확히 이해한다.


# 169. Majority Element

## Solution
```python
class Solution(object):
    def majorityElement(self, nums):

        nums_map = Counter(nums)
        major_element = max(nums_map, key=nums_map.get)
        
        return major_element
```

## Key Takeway
해시맵의 요소로 먼저 nums의 값들을 쫙 깔아줘야 한다.
defaultdict를 이용하면 찾는 값이 key에 없어도 에러를 내지않고 자동으로 추가한다.
그리고 for문으로 nums의 각 요소들을 깔아주는데, 이 때 i는 포인터가 아닌 실제 요소 값이어야 하고, 이에 대해 동일한게 나오면 1씩 더하도록 코드를 짠다.

그 후에 해시맵의 모든 요소를 한 번 순회하면서 majority_value보다 값이 큰 key를 return하면 끝.

# 189. Rotate Array

## Solution
```python
class Solution(object):
    def rotate(self, nums, k):
```

## Key Takeway
