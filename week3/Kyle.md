
# 121. Best Time to Buy and Sell Stock

## Solution
```python
class Solution(object):
    def maxProfit(self, prices):
        """
        :type prices: List[int]
        :rtype: int
        """

        min_price = float("inf")
        max_profit = 0
        i = 0
        while (i < len(prices)):
            if prices[i] < min_price:
                min_price = prices[i]
            elif prices[i] - min_price > max_profit:
                max_profit = prices[i] - min_price
            i += 1
        return max_profit
```

## Key Takeway
배열 안의 수가 정렬되어있지 않기에 투 포인터는 사용하지 못한다. 그러나 모든 것을 한 번의 순회에 이루기 위해서,
모든 가격 지점이 '사는 시점'이거나 '파는 시점' 둘 중 하나로 나뉜다는 것을 이해하고 두 의사결정을 한 번에 할 수 있음을 파악해야 한다.
min_price의 초기값을 부동소수점 최대값으로 잡아 언제나 첫 가격이 min_price 즉 사는 가격이 되도록 처리한다.
이후에는 순회를 돌면서 그 가격을 새로운 '사는 가격'으로 삼을지 아니면 새로운 '팔 가격'으로 삼을지만 결정하면 된다.



# 122. Best Time to Buy and Sell Stock II

## Solution
```python
class Solution(object):
    def maxProfit(self, prices):
        profit = 0
        buy = prices[0]
        for i in range(1, len(prices)):
            if buy < prices[i]:
                profit += prices[i] - buy
            buy = prices[i]
        return profit
```

## Key Takeway
이 문제는 위 문제와 동일하지만 단지 구입과 판매를 여러번 할 수 있다는 조건이 붙을 뿐이다. 그러나 그렇게 됨으로써
의외로 문제는 간단해지는데, 왼쪽부터 오른쪽으로 가면서 이익이 될 수 있는 거래만 챙긴 뒤 그 이익을 더하면 되는 것이다.
이 케이스는 결국 local optimum의 합이 global optimum이라는 전제가 만족된다. 이러면 안전하게 Greedy algorithm을 쓸 수 있다.
Greedy algorithm을 쓰기 위해서는 (반복이 안되더라도) 최적 부분 구조가 존재해야하며, 한 번 내린 결정은 다시 돌아보지 않아야 한다.
이 경우 우리는 왼쪽에서 오른쪽으로 가며 각 부분에 이루어지는 거래에서 얻는 이득이 그 안에서 최적이기도 하다. 

이론적 베이스는 장황하지만 결국, 위와 같이 작은 이익을 더해감으로써 최종 이익을 얻을 수 있다.



# 55. Jump Game

## Solution
```python
class Solution:
    def canJump(self, nums: List[int]) -> bool:
        lastPos = len(nums) - 1
        for i in range(len(nums) - 1, -1, -1):
            if i + nums[i] >= lastPos:
                lastPos = i
        return lastPos == 0

```

## Key Takeway

이건 동적 계획법으로 푸는 문제다. 
보통 아래 4단계를 거쳐 차례대로 풀면 된다.

# Start with the recursive backtracking solution
이건 그냥 brute force solution을 말한다.
모든 경우의 수를 조사하는 것. 그러나 여기에서도 재귀호출이 쓰인다.

class Solution:
    def canJumpFromPosition(self, position, nums):
        if position == len(nums) - 1:
            return True
        furthestJump = min(position + nums[position], len(nums) - 1)
        for nextPosition in range(position + 1, furthestJump + 1):
            if self.canJumpFromPosition(nextPosition, nums):
                return True
        return False

    def canJump(self, nums):
        return self.canJumpFromPosition(0, nums)

기본적인 아이디어는 매 position마다 가장 멀리 갈 수 있는 furthestJump를 구한 뒤, 현재 위치 + 1과 furthestJump + 1 위치 사이에서 재귀호출을 통해 값을 구하는 것이다. 무수히 깊은 재귀 호출 트리에서 한 번이라도 True가 나오면 전체 결과가 True가 나오지만 그러지 못하면 False가 나온다.
이 때 next position을 왼쪽부터가 아니라 오른쪽부터 체크하면 이론상으론 똑같아도 실질적으론 더 빨라진다. 즉 nextPosition을 furthestJump에서 시작해서 하나씩 내려오면 된다는 것.

어쨌든 이러면 시간복잡도가 2^n이 나와서 아주 느려진다(정확히는 2보다 작은 어떤 수의 n거듭제곱에 근사한다). 여기서 N은 배열의 길이다. 공간복잡도는 재귀 호출 스택만큼인 O(N)이 된다.

# Optimize by using a memoization table (top-down dynamic programming)

그러나 위와 같이 매 인덱스마다 유효한지 아닌지를 판단할 때, 한 번 유효하지 않은 인덱스라고 판단되면 다른 재귀 호출 트리에서 여기를 굳이 다시 방문할 필요가 사라진다. 따라서 메모이제이션을 적용해본다. 즉, 탑다운 동적계획법을 써보는 것.

class Solution:
    def __init__(self):
        self.memo = []
        self.nums = []

    def canJumpFromPosition(self, position):
        if self.memo[position] != -1:
            return self.memo[position] == 1
        furthestJump = min(position + self.nums[position], len(self.nums) - 1)
        for nextPosition in range(position + 1, furthestJump + 1):
            if self.canJumpFromPosition(nextPosition):
                self.memo[position] = 1
                return True
        self.memo[position] = 0
        return False

    def canJump(self, nums):
        self.nums = nums
        self.memo = [-1] * len(nums)  # -1 for unknown, 0 for bad, 1 for good
        self.memo[-1] = 1  # The last position is always "good"
        return self.canJumpFromPosition(0)

memo를 배열과 같은 길이로 만들어두고 전부 -1을 넣어둔 뒤 -1이 아닌 다른 값이 저장되어있으면 그걸 사용한다.
그 외의 로직은 같다. 이미 방문한 포지션인데 if문 조건에 해당되어 True라면 그 사실을 1이라는 값으로 저장한다.

이러면 시간복잡도가 O(N^2)가 나온다. 공간복잡도는 재귀 깊이로 인한 N과 메모때문에 생기는 N으로 2N이 되니 O(N)이다.


# Remove the need for recursion (bottom-up dynamic programming)

이제 Top-down DP에서 Bottom-up DP로 바꿔보자. 타뷸레이션 기법을 사용하는 바텀업 DP는 재귀가 아닌 iteration을 통해 bottom-up으로 문제를 해결하며, 데이터는 주어진 배열이나 테이블(위 예시에선 그냥 cache라는 이름의 배열)에 저장한다. 재귀 스택에 들어가는 연산을 아끼므로 모든 경우를 다 평가할 때는 바텀업이 탑다운 DP보다 빠르다. 또 재귀를 없앤 것만으로도 추가적인 최적화를 가능케 하기도 한다.

이 바텀업으로의 전환이 가능한 것은, 이 문제가 반드시 오른쪽으로 즉 인덱스가 반드시 더 큰쪽으로 작업이 진행되기 때문이다. 원래 탑다운 DP에서 바텀업 DP로 전환하려면 Directed Acyclic Graph 즉 방향성이 있고 순환이 없는 (트리같은) 구조여야 한다. 그리고 위상 정렬이 가능(어떻게 정렬해도 왼쪽부터 시작해서 차례대로 하면 모든 작업을 완수 가능)해야 한다. 점프 위치의 인덱스가 한쪽에서 다른쪽으로 쭉 커지는 경우 이 모든 조건을 만족하므로 시도해볼 수 있다. 

우리가 왼쪽에서 오른쪽으로 점프한다는 점은, 오른쪽에서 조사를 시작하면 조사 위치의 우측에 위치한 요소들을 조사할 때는 이미 조사를 완료되어있을 것이고, 따라서 이를 캐싱해두면 큰 이득을 볼 수 있다는 것이다. 이러면 재귀를 안해도 된다. 

class Solution(object):
    def canJump(self, nums):
        GOOD, BAD, UNKNOWN = 1, 0, -1
        memo = [UNKNOWN] * len(nums)
        memo[-1] = GOOD
        for i in range(len(nums) - 2, -1, -1):
            furthest_jump = min(i + nums[i], len(nums) - 1)
            for j in range(i + 1, furthest_jump + 1):
                if memo[j] == GOOD:
                    memo[i] = GOOD
                    break
        return memo[0] == GOOD
			
표지를 가능, 불가능, 미확인 3 상태로 놓고, 가장 마지막 요소는 항상 가능하므로 GOOD으로 맞춰둔다. 그리고 끝에서부터 시작해서 한 칸씩 왼쪽으로 이동해가며 앞에서 본 같은 로직 즉 두 경우 중 최소값으로 점프 가능 여부를 판단하는 반복문을 작성한다. 
그런데 이렇게 해도 시간 복잡도는 여전히 O(N^2)이고 공간복잡도도 O(N)이다. 


#Apply final tricks to reduce the time / memory complexity

이제 여기서 더 최적화를 가려면, 그리디 알고리듬을 적용할 구석을 찾는다. 그리디 알고리듬을 마음 편하게 쓰려면 한 번 내린 결정을 뒤돌아보지 않고 앞으로 진행해도 된다는 확신, 즉 로컬 옵티멈이 글로벌 옵티멈이라는 확신이 있어야 한다. 위 코드를 보면, 어떤 위치 i가 GOOD인지 확인하기 위해 이 다음 어딘가에 있는 첫 번째 GOOD을 j로 탐색하고 있다. i기준 그 오른쪽으로 쭉 j들을 찾다가 한 번이라도 GOOD이 나오면 이 i는 곧바로 GOOD이 되어버린다. 이 말인 즉, 만약 어떤 지점 i를 기준으로 그 오른쪽에 있는 요소중 가장 왼쪽 즉 처음 나타나는 GOOD 위치를 별도의 변수로 저장해둔다면, 이 뒤로 요소 순회가 일어나는 것을 막을 수 있다. 이러면 배열 자체를 안 써도 된다. 

GOOD인지 확인하는 매 위치마다 그 오른쪽에 이미 알려진 GOOD 위치로 점프가 가능한지를 판단하는 것은 아래와 같이 가능하다:
currPosition + nums[currPosition] >= leftmostGoodIndex
즉 이 조건만 만족하면 바로 현재 위치는 GOOD인 것. 그리고 이 GOOD 위치는 새로운 '가장 왼쪽의' GOOD 즉 leftmostGoodIndex가 된다. 

그래서 이 코드는 아래와 같이 극한으로 최적화된다:

class Solution:
    def canJump(self, nums: List[int]) -> bool:
        lastPos = len(nums) - 1
        for i in range(len(nums) - 1, -1, -1):
            if i + nums[i] >= lastPos:
                lastPos = i
        return lastPos == 0
				
이러면 시간복잡도는 O(N)이다. 원패스로 해결하기 때문. 공간복잡도는 아예 캐싱조차 안하므로 O(1)이다. 
