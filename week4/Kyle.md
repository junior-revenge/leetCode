
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
class Solution:
    def hIndex(self, citations):
        n = len(citations)
        papers = [0] * (n + 1)
        # Count how many papers have each citation count (cap at n)
        for c in citations:
            papers[min(n, c)] += 1

        # Find the h-index
        k = n
        s = papers[n]
        while k > s:
            k -= 1
            s += papers[k]
        return k

```

## Key Takeway
우선 brute force로 다가가봄직하다. 이게 h 인덱스 계산하는 방식이 좀 특이해서 시행착오는 겪겠지만, 결국 최대값을 찾고, 그 값이 배열 안에서 몇 번 나타나는지를 확인하며 순회하면 된다.
이 때 놓치기 쉬운 것이, max값이 당장 그 값만큼 나타나지 않으면 바로 잊을 수 있는데, [11, 15]의 케이스처럼 배열 안에 없는 값이라도 h인덱스가 될 수 있다. 이 경우엔 2가 될 것.
그래서 h인덱스가 될 값 자체는 배열에 없더라도 1씩 줄여가며 계속 조사해야한다.

```python
class Solution(object):
    def hIndex(self, citations):
        original_array = citations[:]
        max_val = max(citations)
        while max_val > 0:
            count = len([num for num in original_array if num >= max_val])
            if count >= max_val:
                return max_val
            if max_val in citations:
                citations.remove(max_val)
            max_val -= 1
        return 0
```

최종적으로 이렇게 하면 문제는 풀 수 있다. 시간복잡도는 O(N)이 된다. 순회는 한번만 이루어지고 max값을 찾는 것은 max값만큼의 연산을 하면 되므로 O(max * N)이 되는데 그래서 O(mN)이다. 공간 복잡도는 배열 하나를 따로 관리하므로 역시 O(N)이다.

이걸 수학적으로 접근하면, 그래프를 그려보면 x축에 논문의 수, y축에 인용횟수를 놓고 봤을 때 정사각형을 그릴 수 있는 최대의 변의 길이 h를 구하는 것과 같다. 따라서 배열을 내림차순(오름차순이면 뒤에서부터 조사하면 됨)으로 정렬한 후 그 너비가 최대가 되는 정사각형의 한 변의 길이를 구해도 된다.

그렇게 하면 인용횟수가 인덱스보다 큰 인덱스까지 셈을 한 뒤에 거기에 1을 더한 값(인덱스는 0부터 시작이니까 i + 1)을 h인덱스로 산출하면 된다. 
이렇게 하면 나머지 논문들(n - (i + 1))은 인용횟수가 i+1보다 작아야만 한다.

그래서 sorting에 시간복잡도를 조금 소모한 후 전체 배열을 각 인덱스에 대해 위 조건에 해당하는지만 따져서 더한 뒤 반환하면 끝이다.

```python
class Solution:
    def hIndex(self, citations):
        # Sort the citations in ascending order
        citations.sort()
        i = 0
        n = len(citations)
        # Linear search from the end
        while i < n and citations[n - 1 - i] > i:
            i += 1
        return i

```
이러면 시간복잡도가 O(NlogN)이 나온다. 이게 O(mN)보다 빠르다. m이 굉장히 커질 수 있으므로. 물론 정렬 후 binary search로 값을 찾을 수도 있는데 이래봐야 O(NlogN)을 더 빠르게는 못만든다. 애초에 정렬할 때 O(NlogN)보다 빠를 수가 없어서.
<img width="1400" height="1000" alt="image" src="https://github.com/user-attachments/assets/5bca4f07-15d2-495b-b9c8-449ab728bbac" />

그래서 이걸 더 최적화하려면 정렬 알고리듬을 건드려야 한다. Heap 정렬이나 Merge 정렬, Quick 정렬등은 아무리 빨라도 O(NlogN)보다 빨라질 수 없다. 그래서 새로운 정렬법인 counting 정렬을 사용한다. 이 정렬은 자주 쓰이진 않지만 integer한해서 비교없이 정렬이 가능하기에 배열 속 정수의 값이 너무 크지만 않으면 대체로 빠르다.
카운팅 정렬은 기본적으로 고유한 값에 어떤 수식을 적용해 이 값이 위치해야하는 곳의 인덱스를 구해 그리로 바로 옮기는 방식으로 정렬한다. 다만 우리의 데이터는 논문의 수보다 논문이 가진 인용횟수가 훨씬 클 수 있다는 문제점이 있어서 이 정렬이 효율적이지 않을 수 있다.

그러나, 우리는 면적이 최대가 되는 정사각형만 찾으면 되기때문에, 인용횟수가 n보다 큰 논문들은 그 인용횟수를 n이라고 치환해버려도 h 인덱스 계산에 영향을 안미치게 된다. 그러면 앞에서 정렬로직을 이걸로 치환하면 더 나은 해법을 얻게 된다.
<img width="714" height="617" alt="image" src="https://github.com/user-attachments/assets/6c54a61e-8205-4779-8b3b-8f2f30da84fc" />

하지만 여기서 한 발자국 더 나아갈 수도 있다. 만약 우리가 치환이 가능하단걸 깨달았다면, 여기서 정렬 자체를 건너뛸 수도 있다. 왜냐하면 인용횟수에서 바로 h인덱스를 구할 수 있기 때문이다. h인덱스는 어차피 0에서 citations 배열의 길이 사이의 값일 것이다. 논문 갯수보다 h인덱스가 커질 수는 없기 때문. [1 3 2 3 100] 또는 치환해서 [1 3 2 3 5] 이렇게 있다고 할 때, h인덱스가 될 수 있는 0에서 5사이의 값을 i, 그리고 그 i보다 더 크거나 같은 인용횟수를 가진 논문의 갯수의 합을 s(i)라고 정의했다고 치자. 그러면 s(i)의 값은 i가 5일 때는 1일 것이다. 5보다 크거나 같은 논문이 하나니까. s(4)의 값은 인용횟수가 4보다 큰 논문이니까 여전히 1일 것이다. 인용횟수가 3보다 큰 논문의 갯수인 s(3)는 3일 것이다. s(2)는 4, s(1)은 5 그리고 s(0)도 5일 것이다.

우리의 h인덱스는 이 i들 사이에서 s(i)보다 작거나 같은 i중 가장 큰 숫자다. 즉 i는 0부터 쭉 무한히 커지고, s(i)는 최대 5에서 시작해서 5, 4, 3, 1, 1 ... 0 이렇게 쭉 작아지니까 i가 s(i)보다 작거나 같은 경우는 i가 0일때 s(i)가 5, i가 1일 때 s(i)가 5, i가 2일 때 s(i)가 4, i가 3일 때 s(i)가 3이고 이 뒤로는 i가 커져도 s(i)는 계속 작아진다. 즉 우리의 h인덱스는 3이다.

이러면 정렬이고 나발이고 필요없다. 그냥 원패스로 계산 가능.

```python
class Solution:
    def hIndex(self, citations):
        n = len(citations)
        papers = [0] * (n + 1)
        # Count how many papers have each citation count (cap at n)
        for c in citations:
            papers[min(n, c)] += 1

        # Find the h-index
        k = n
        s = papers[n]
        while k > s:
            k -= 1
            s += papers[k]
        return k
```
이러면 압도적 시간복잡도 O(N)이 나온다.
다르게 설명하면, 
우리가 n+1개의 0으로 가득찬 papers를 만드는 이유는 n보다 큰 인용수는 그냥 n으로 치환하고 말것이기 때문이다.
각 인용횟수 c당 c가 n보다 커도 우리는 그걸 n으로 치환한다. 따라서 min()함수를 쓰고 있다. 그리고 해당 인용횟수가 나타날 때마다 카운트 값을 1씩 올린다.
만약 citation이 [3 0 6 1 5]라면 papers는 [1 1 0 1 0 2]가 된다. 
이건 별게 아니고 그냥 인용횟수 0인 논문 1개, 1인 논문 1개, 3인 논문 1개, 5이상인 논문 2개라는 뜻에 불과하다. 
이래놓고 h index를 찾는다. 우리가 가진 값중 가능한 가장 높은 h 인덱스는 5이므로(논문이 다섯개) 거기서부터 시작하기 위해 
k = n 으로 놓은 것이다. 그리고 여기서 거꾸로 하나씩 줄여가며 확인한다. 우리는 앞서 설명에서처럼 특정 인용횟수를 가진 논문의 갯수 중에 5보다 작은 것중 가장 큰걸 골라야 하기 때문.
그래서 큰거에서 시작해서 줄여나가는 것. k가 누적합 s와 같거나 작아지기 시작하면 합산을 멈추고 그값을 반환한다.


# 380. Insert Delete GetRandom O(1)

## Solution
```python
class Solution:

```

## Key Takeway
f
