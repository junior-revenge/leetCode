
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