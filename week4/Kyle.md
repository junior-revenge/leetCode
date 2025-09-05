
# 45. Jump Game II

## Solution
```python
class Solution:
    def jump(self, nums: List[int]) -> int:
        # The starting range of the first jump is [0, 0]
        answer, n = 0, len(nums)
        cur_end, cur_far = 0, 0

        for i in range(n - 1):
            # Update the farthest reachable index of this jump.
            cur_far = max(cur_far, i + nums[i])

            # If we finish the starting range of this jump,
            # Move on to the starting range of the next jump.
            if i == cur_end:
                answer += 1
                cur_end = cur_far

        return answer
```

## Key Takeway
이러한 유형의 문제는 당연히 반드시 n - 1에 도착할 수 있는 배열이라는 것을 전제로 한다.
기본적으로 트리 구조로 이해를 하였고, 마지막 인덱스에 도달하면 종료되는 경우의 수를 인덱스를 노드값으로 하여 트리를 그려보면,
BFS로 접근하면 풀 수 있다는 사실을 알 수 있다. 하여 아래와 같이 코드를 작성.
```python
class Solution(object):
    def jumpRecursive(self, nums, index, depth):
        n = len(nums)
        q = deque([(index, depth)])
        visited = set([index])
        while q:
            i, d = q.popleft()
            if i == n - 1:
                return d
            limit = min(n, i + nums[i] + 1)
            for j in range(i + 1, limit):
                if j == n - 1:
                    return d + 1
                if j not in visited:
                    visited.add(j)
                    q.append((j, d + 1))
        return 0
        
    def jump(self, nums):
        return self.jumpRecursive(nums, 0, 0)
```
(만약 DFS로 풀었다면 메모이제이션을 적용해줘야 겨우 해결이 가능한 정도고 그마저도 O(N^2)로 굉장히 느리다.)

그러나, 이 문제는 특성상 반드시 last index에 도달할 수 있기에 각 인덱스에서의 점프 가능 최대 지점은 언제나 이전 인덱스의 그것보다 크다. 이런 부분들을 활용하여, 언제나 그렇듯, 그리디 알고리듬으로 튼다.

여기서 그리디 알고리듬으로 튼다함은, 새롭게 방문해야하는 인덱스만 방문한다는 것. 예를 들어 i에서 저픔구간이 [0, 2]라면
i + 1에서 [0, 4]를 조사할 수 있어도 [3, 4]만 조사하겠다는 식. 이를 구현하기 위해서는 현재 점프의 가장 멀리 있는 starting index와 현재 점프에서 가장 멀리 도달할 수 있는 인덱스를 저장해 관리해야한다.

현재 인덱스를 기준으로 순회를 하다가 '가장 멀리있는 starting index'에 도달하면, 그 다음부턴 그보다 큰 인덱스만 조사하면 된다. 

그걸 반영하면 one pass로 해결할 수 있고 시간복잡도도 BFS방식의 O(N^2)이 아닌 O(N)으로 끝낼 수 있다.


# 274. H-Index

## Solution
```python
class Solution(object):

```

## Key Takeway
이 


# 380. Insert Delete GetRandom O(1)

## Solution
```python
class Solution:

```

## Key Takeway
f