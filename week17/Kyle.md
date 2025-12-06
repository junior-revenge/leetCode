
# 1. Two Sum

## Solution
```python
class Solution(object):
    def twoSum(self, nums, target):
        index_map = {}
        for i, num in enumerate(nums):
            comp = target - num
            if comp in index_map:
                return [index_map[comp], i]

            index_map[num] = i
```

## Key Takeway
해시맵을 활용하고, 미리 등록해놓은 뒤, complement를 매번 구해서, 그게 맵에 있는지 잘 확인해서 반환하면 된다.
그리고 인덱스가 겹치지 않도록 현재 순회중인 i와 map에 complement를 key로 갖고있는 항목의 value인 인덱스가 같지 않은지 체크하면 된다.

원패스로 하려면 반드시 complement를 먼저 구해서 있는지 확인하고 없으면 해시맵에 집어넣어야 한다.
# 200. Happy Number

## Solution
```python
class Solution(object):
    def isHappy(self, n):
        memory = defaultdict(int)
        while n != 1:
            sum_of_squares = 0
            while n > 0:
                i = n % 10
                sum_of_squares += i * i
                n = (n - i) / 10
            if sum_of_squares in memory:
                return False
            memory[sum_of_squares] += 1
            n = sum_of_squares 

        return True
```

## Key Takeway
문제를 보면, 일단 아래와 같은 로직으로 숫자를 자릿수 단위로 쪼개서 더할 수 있다.
1)n이 0보다 클 때
2)n을 10으로 나눈 나머지를 제곱해서 결과값에 더하고
3)n에서 그 나머지를 뺀 값을 10으로 나눈 값을 새 n으로 설정한다.

그리고 이제 이걸 계속 반복해야 하므로,
1)n이 1이 아니라면
2)각 자릿수의 제곱을 더한 값을 0으로 초기화한 후
3)nested while문으로 위의 작업을 한 뒤 
4)더한 결과값을 새로운 n으로 선언한다. 

그러면 이 과정을 빠져나온 n에 대해서는 True를 반환할 수 있을 것이다.
그러나 1로 영영 가지 못하는 수가 있다. 2의 예시를 가지고 숫자를 한번 굴려보면, 정해진 숫자가 계속해서
반복된다는 것을 알 수 있다. 따라서 이를 해결하려면 각 자릿수의 제곱을 더한 결과값이 이전에 나왔던 값인지
아닌지 알아내야 한다.

따라서 해시맵 변수를 하나 추가하고,
결과값에 각 자릿수의 제곱값을 더해가는 작업이 끝나고 나면 그걸 해시맵에 저장한다.
그리고 그 해시맵 저장이 이루어지기 직전에 이미 해시맵에 있는지 확인한다.
여기서 이미 있으면 바로 False를 반환하면 된다. 


여기까지 풀면 충분하다. 

# 219. Contains Duplicate II

## Solution
```python
class Solution(object):
    def containsNearbyDuplicate(self, nums, k):
        last = {}

        for i, num in enumerate(nums):
            if num in last and i - last[num] <= k:
                return True
            last[num] = i

        return False

```

## Key Takeway

Solution이라고 제공된 것과는 좀 다르게 접근했는데, 어찌되었든 해시 테이블을 사용하면 충분히 
최적화된 해법으로 풀 수 있다. 결국 순회를 하면서 지나온 값들의 인덱스 정보를 필요한 형태로 저장하고
인출할 때 해시맵을 쓰면 되니까.

그래서 일단 해시맵이 필요하다고 생각했고, nums를 돌리면서 맵에 없으면 인덱스를 저장했다.
그리고 거기서 더 뭘 하면 안되므로 continue를 불러줬고, 그걸 통과하면 이제 핵심 로직인 거리가 k이내인지를 확인했고
이내면 참을 반환. 
그런데 이렇게만 하면 같은 숫자가 여러번 나올 경우 처음 나온 시점의 인덱스만 기억하게 되어서 정확히 답을 낼 수가 없는데,
그래서 이렇게 조건에 만족하지 못했지만 한번 나왔던 인덱스의 경우 새로 업데이트를 꼭 해주도록 했다.

```python
class Solution(object):
    def containsNearbyDuplicate(self, nums, k):
        index_map = defaultdict(int)

        for index, num in enumerate(nums):
            if num not in index_map:
                index_map[num] = index
                continue
            
            if abs(index_map[num] - index) <= k:
                return True
            index_map[num] = index
        
        return False
```
그래서 이렇게 풀면 충분히 시간복잡도와 공간복잡도 O(N)이 된다. 이 로직을 간소화 하면 다음과 같다: 
```python
class Solution(object):
    def containsNearbyDuplicate(self, nums, k):
        last = {}

        for i, num in enumerate(nums):
            if num in last and i - last[num] <= k:
                return True
            last[num] = i

        return False
```