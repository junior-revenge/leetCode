
# 88. Merge Sorted Array

## Solution
```python

class Solution(object):
    def merge(self, nums1, m, nums2, n):
        while n > 0:
            if nums2[n-1] > nums1[m-1] or m == 0:
                nums1[m+n-1] = nums2[n-1]
                n -= 1
            else:
                nums1[m+n-1] = nums1[m-1]
                m -= 1


```

## Key Takeway
기본적으로 두 가지:

1) 뒤에서 시작할 것. 앞에서 시작하면 내가 아직 판단하지 않은 데이터를 자꾸 뒤에 붙여넣어야 하니 걸리적 거린다. 그래서 끝에서 시작한다. 이 경우 작업을 한 단계 해도 nums1의 길이가 변하지 않기에 도입 가능.

2) 포인터 갯수를 정할 것. 보통 포인터는 내가 다루고 있는 구분되는 데이터 소스나 시퀀스의 갯수만큼 필요하다. 특히 둘로 기존 데이터를 합치는 상황에서는 합친 값을 집어넣을 위치에 대한 포인터도 필요하다. 여기선 결국 포인터 세개를 활용하는데, 그 세개가 다 m과 n으로 표현이 가능하다.


# 27. Remove Element

## Solution
```python

class Solution(object):
    def removeElement(self, nums, val):
        second_pointer = 0
        for i in range(len(nums)):
            if nums[i] != val:
                nums[second_pointer] = nums[i]
                second_pointer += 1
        return second_pointer

```

## Key Takeway
인플레이스로 구현해야하기에 별도의 배열을 추가로 사용할 순 없다.

단순한 방식으로는 배열을 먼저 조사해서 value를 제거하고 발견할 때마다 모든 나머지 배열을 한 칸 왼쪽으로 옮기는 식으로 작업했어야 할 것이다.
그러면 n^2로 작업 가능.

그러나,
1) 순서를 유지할 필요가 없고
2) val과 일치하지 않는 요소만 신경쓰면 되므로

투포인터를 적용시도해볼 수 있다.

첫 포인터는 array를 따라 쭉 움직이고
두 번째 포인터는 "이 다음에 올 val과 일치하지 않는 요소가 어디에 위치해야하는지에 대한 정보"를 나타내야 한다. 예를 들어 첫번째 요소가 val과 일치하지 않는다면, 그 다음에 val과 일치하지 않는 요소를 또 발견했을 경우, 이 첫번째 요소 바로 뒤에 놓여져야 한다는 정보를 저장하는 것이다.

# 26. Remove Duplicates from Sorted Array

## Solution
```python
class Solution(object):
    def removeDuplicates(self, nums):
        second_pointer = 1
        for i in range(len(nums)-1):
            if nums[i] != nums[i+1]:
                nums[second_pointer] = nums[i+1]
                second_pointer += 1
        return second_pointer
```

## Key Takeway
이전 문제와 마찬가지로 투포인터를 떠올릴 수 있다.

시작을 했을 때, 이 다음 값과 같은지 비교해서,

1) 같으면 그냥 넘어간다.
2) 다르면 그 값을 가져오고 고유한 값의 수를 나타내는 두번째 포인터의 값을 하나 올린다. 이 값은 다음 고유한 값의 위치이기도 하다.

두번째 포인터는 1에서 시작하더라도 언제나 인덱스 역할의 첫 번째 포인터보다 느리고, 배열은 정렬되어있으므로, 두 번째 포인터의 위치에서만 값을 업데이트해주더라도 전혀 문제가 발생하지 않는다.
