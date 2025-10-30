
# 11. Container With Most Water

## Solution
```python
class Solution(object):
    def maxArea(self, height):
        # simple two pointer question
        max_area = 0
        i = 0
        j = len(height) - 1
        while (i < j):
            max_area = max(max_area, (j - i) * min(height[i], height[j]))
            if height[i] < height[j]:
                i += 1
            else:
                j -= 1

        return max_area
```

## Key Takeway

bruteforce로 하면 모든 인덱스의 조합을 구해야하고 nC2 즉 n(n-1)/2가 되므로 O(N^2)의 시간복잡도가 나올것이다.

그래서 이걸 원패스로 하기 위해서는 포인터를 움직이는 로직을 건드려야 하는데,
잘 생각해보면, 일단 너비를 결정하는 변수는 두 포인터 사이의 간격과 각 포인터위치에서의 높이값이다.

여기서 어쩌면 떠올리는게 쉬운 "두 포인터 위치의 높이값중 더 낮은 값을 가진 쪽의 포인터를 안쪽으로 진행시킨다"는 로직은
그리디한 접근법인데, 이게 최적값을 줄거라는 확신을 갖기는 어려울 수 있다.

결론부터 말하면, 짧은 쪽(i)을 유지한 채 다른 쪽을 안으로 옮기는 모든 조합은 현재보다 절대 좋아질 수 없ek. 높이가 낮은 벽을 유지하면서 다른 포인터를 움직이는 순간, 그 경우에 잡히는 모든 물용량은 절대 기존의 용량보다 커질 수 없다는 점을 인지하면 된다. 그 사실만 깨우치면 그리디한 접근을 확신을 갖고 쓸 수 있다. 

높이(두 포인터의 높이값중 작은 값)와 너비(두 포인터 사이의 거리)중 높이만 놓고 보자.
물의 양에 있어서 높이는 반드시 두 포인터 중 가장 낮은 높이값을 가진 포인터를 기준으로 결정된다. 즉 i와 j가 있을 때, j가 i보다 훨씬 큰 상황에서, i와 j사이에 혹시 있을지 모를 k를 가정해보자. 이 k가 설령 j보다 100만배 크더라도, 결국 물의 최대높이는 j보다 높이값이 낮았던 i를 기준으로 결정된다. k의 높이값이 100만배 크더라도 i의 높이값만큼만 물이 차기 때문. 
그런데 또다른 변수인 두 포인터 사이의 너비 즉 폭은 어떠한가? 이건 (높이는 같다고 가정하고) 포인터를 안쪽으로 가져올때마다 반드시 비례적으로 물의 용량을 줄인다.
높이라는 변수는 둘중 어느 포인터가 작으면 그쪽에 의해 100% 결정되는데 반해, 너비는 양끝 포인터를 안쪽으로 가져올수록 비례적으로 손해를 본다. 결국 두 포인터 사이의 간격을 하나 줄인다고 잃는 손해는 모든 과정에 고루 분산되어있는 반면, 포인터 높이를 더 낮은 것을 고름으로 인해서 입는 손해는 비선형적이며 임팩트가 월등히 크다.

따라서 그리디하게 양끝에서 시작해서 포인터를 가깝게 해서 잃는 손해보다 낮은 포인터를 고수함으로 인해 얻는 손해가 더크고, 그리디하게 접근해도 최적값을 찾을 수 있으리란 확신을 할 수 있다.

# 15. 3Sum

## Solution
```python
class Solution(object):
    def threeSum(self, nums):
        nums.sort()
        n = len(nums)
        ans = []

        for i in range(n):
            if i > 0 and nums[i] == nums[i-1]:
                continue  # skip same pivot

            target = -nums[i]
            l, r = i + 1, n - 1

            while l < r:
                s = nums[l] + nums[r]
                if s == target:
                    ans.append([nums[i], nums[l], nums[r]])
                    l += 1
                    r -= 1
                    # skip duplicate l/r
                    while l < r and nums[l] == nums[l-1]:
                        l += 1
                    while l < r and nums[r] == nums[r+1]:
                        r -= 1
                elif s < target:
                    l += 1
                else:
                    r -= 1

        return ans
```

## Key Takeway
각 요소에 대해 투썸을 해버리면 된다.
투썸이 O(N)이니까 이러면 O(N^2)가 되는데, 시간복잡도 자체는 여기서 더 개선이 어렵다.


class Solution(object):
    def threeSum(self, nums):
        ans = []
        seen = set()
        
        for i in range(len(nums)):
            outer_target = 0 - nums[i]
            remaining = nums[i + 1:]
            map = {}
            for j in range(len(remaining)):
                map[remaining[j]] = j
            for j in range(len(remaining)):
                target = outer_target - remaining[j]
                if (
                    target in map and
                    i != i + j + 1 and
                    i != i + map[target] + 1 and
                    i + j + 1 != i + map[target] + 1
                ):
                    triplet = sorted([nums[i], remaining[j], target])
                    triplet_tuple = tuple(triplet)
                    if triplet_tuple not in seen:
                        seen.add(triplet_tuple)
                        ans.append(triplet)

        return ans
       
			 
단, 이렇게만 하면 문제가 좀 있는게, 중간에 i가 i+j+1과 다른지 확인하는 부분과 i가 i+map[target]+1이랑 다른지 확인하는 부분은 불필요하다. 왜냐하면 j와 map[target] 값이 언제나 0 또는 양의 정수기 때문. i를 제외한 나머지 둘이 다른지만 확인하면 된다.

그리고 중간에 remaining잡을 때 slicing을 써서 거기서 O(N)이 추가되어버린다.

따라서 시간복잡도는 개선이 어려워도 더 세련된 풀이는 전체를 한 번 정렬한 후 포인터 두개만 쓰는 것이다.
```python
class Solution(object):
    def threeSum(self, nums):
        nums.sort()
        n = len(nums)
        ans = []

        for i in range(n):
            if i > 0 and nums[i] == nums[i-1]:
                continue  # skip same pivot

            target = -nums[i]
            l, r = i + 1, n - 1

            while l < r:
                s = nums[l] + nums[r]
                if s == target:
                    ans.append([nums[i], nums[l], nums[r]])
                    l += 1
                    r -= 1
                    # skip duplicate l/r
                    while l < r and nums[l] == nums[l-1]:
                        l += 1
                    while l < r and nums[r] == nums[r+1]:
                        r -= 1
                elif s < target:
                    l += 1
                else:
                    r -= 1

        return ans
```
위 해법에서 조금 헷갈릴 수 있는건, 왜 target을 정확히 짚어냈을 때 left와 right을 동시에 움직이느냐 하는 것인데,
이게 정렬이 되어있기 때문에, target을 정확히 찾아낸 시점에서 한쪽만 고정하고 다른쪽만 바꾸면 반드시 target값에 부족하거나 지나치게 된다.
예를 들어 target이 5인데 left가 1 right가 4라면, 여기서 left만 오른쪽으로 이동하면 반드시 5보다 큰 값이 나온다(1이 그대로 나와서 똑같은 5가 나올 수 있지만 다른 줄에서 중복은 제거하고 있다), 마찬가지로 left를 1에 두고 right만 왼쪽으로 이동시켰는데 그값이 3이었다면 반드시 5보다 작은 값이 나온다. 중복이 아닌한. 
그래서 target을 이미 맞춘 이후에는 l을 오른쪽으로 밀어서 값을 높였다면 반드시 r을 왼쪽으로 밀어서 값을 낮춰줘야 그 둘의 합이 target이 되는 다른 조합을 찾을 가능성이 있게 된다.
            


# 209. Minimum Size Subarray Sum

## Solution
```python
class Solution:
    def minSubArrayLen(self, target, nums):
        left = 0
        right = 0
        sumOfCurrentWindow = 0
        res = float('inf')

        for right in range(0, len(nums)):
            sumOfCurrentWindow += nums[right]

            while sumOfCurrentWindow >= target:
                res = min(res, right - left + 1)
                sumOfCurrentWindow -= nums[left]
                left += 1

        return res if res != float('inf') else 0
```

## Key Takeway

```python
class Solution(object):
    def minSubArrayLen(self, target, nums):
        min_length = float('inf')
        left = 0
        right = len(nums) - 1
        while left <= right:
            if sum(nums[left:right+1]) < target:
                if nums[left] <= nums[right]:
                    left += 1
                else:
                    right -= 1
            else:
                min_length = min(min_length, right - left + 1)
                if nums[left] <= nums[right]:
                    left += 1
                else:
                    right -= 1

        return 0 if min_length == float('inf') else min_length
```      

이 문제를 이렇게 투포인터로 양쪽에서 좁혀오면서 풀려고 하면,
언뜻 그리디하게 작동하는것 같아도 최적해에 도달하지 못한다. 게다가 계속 subarray의 합을 계산하려고 하면 O(N^2)보다
빠르게 할 수도 없다.

저 해법이 최적해에 도달하지 못하는 이유는 결과적으로 그리디하게 접근했을 때 로컬최적해가 전역최적해가 아니기 때문이다.
아래 예시를 보자:
nums = [16, 12, 1, 18], target = 28
이러면 시작할때는 16 18로 시작할 것이다. 그러면 합이 47이니 더 줄인다고 왼쪽 값(16)이 오른쪽(18)보다 작으니 왼쪽을 뺀다.
그러면 12 + 1 + 18이라서 31이다. 그러면 여기서 다시 28보단 크니 좋다고 왼쪽을 줄일 것이고 이러면 1+18이 되어서 target 도달을 못한다.
이러면 정답이 12, 1, 18 즉 3이라고 내뱉게 된다. 그러나 실제로는 최적해는 16, 12 즉 길이 2이다. 

그래서 이 문제의 의의는 그리디하게 접근했을 때 최적해를 달성하지 못할 것이라는걸 캐치해내지 못하면
풀기 어렵다는 점이다. 웬만한 작은 예시들도 다 위 로직으로 해결은 되기 때문.

그래서 문자든 숫자든 이렇게 하위배열을 가지고 놀아야 할 때는 괜히 투포인터에서 멈추지말고 유연하게 늘리거나 줄일 수 있는
슬라이딩 윈도우 기법을 쓰는게 맞다. 

그리고 합 계산도 매번 하위배열 전체를 계산할 생각하지 말고, 슬라이딩 윈도우의 증감에 따라 같이 더했다 뺐다 해야한다.

최적해의 기본 전략은 오른쪽으로 먼저 쭉 뻗어나가서 target보다 커진 뒤에, 그 다음부터 왼쪽에서 하나씩 줄여오는 것이다.
그러나 왼쪽에서 줄였다가 다시 target보다 작아지면 오른쪽으로 한칸 더 간다.
