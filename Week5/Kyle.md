
# 238. Product of Array Except Self

## Solution
```python
class Solution(object):
    def productExceptSelf(self, nums):
        length = len(nums)
        answer = [0] * length
        answer[0] = 1
        for i in range(1, length):
            answer[i] = nums[i - 1] * answer[i - 1]
        R = 1
        for i in reversed(range(length)):
            answer[i] = answer[i] * R
            R *= nums[i]
        return answer

```

## Key Takeway

나눗셈을 못 쓰므로 단순히 생각했을 때 매 요소마다 좌 우 값을 곱하는 경우를 생각할 수 있다.
이를 위해 매 i번째 요소마다 그 왼쪽의 값과 오른쪽의 값을 전부 곱한 뒤 다시 그 둘을 곱해 결과를 내보자.
```python
class Solution(object):
    def productExceptSelf(self, nums):
        output = [1] * len(nums)
        for i in range(len(nums)):
            left = 1
            right = 1
            for j in range(i):
                left *= nums[j]
            for j in range(i + 1, len(nums)):
                right *= nums[j]
            output[i] = left * right
        
        return output
```
				
그러나 당연히 이렇게 하면 시간복잡도가 O(N^2)가 될 것이다. 여기서 반복이 일어나는 부분은 i에 위치한 요소의 좌우에 위치한 요소들이
여러번 곱해지기 때문에 일어난다. 따라서, 이를 해결하기 위해 미리 곱셈을 해놓고 그 값을 그대로 참조해서 사용하면 시간복잡도를 O(N)으로 줄일 수 있다.
이때 left항목과 right항목의 반복될 인덱스를 정확히 쓰는게 쉽지 않으니 주의할 것.
```python
class Solution(object):
    def productExceptSelf(self, nums):
        left_multiples = [1] * len(nums)
        right_multiples = [1] * len(nums)
        final_output = [1] * len(nums)

        for i in range(1, len(nums)):
            left_multiples[i] = nums[i - 1] * left_multiples[i - 1]

        for i in range(len(nums) - 2, -1, -1):
            right_multiples[i] = nums[i + 1] * right_multiples[i + 1]

        for i in range(len(nums)):
            final_output[i] = left_multiples[i] * right_multiples[i]
        
        return final_output
```		
이러면 시간복잡도가 O(N), 공간복잡도가 O(N)이다.
문제에 보면 공간복잡도를 O(1)로 줄여보라고 하는데 최종 아웃풋 배열은 이 계산에서 고려하지 않는다고 가정한 것이다. 
그래서 Left나 Right을 나타내는 배열을 쓰지 않고 아웃풋 배열 안에서 전부 해결하는 정도로 도전하면 된다.
방법은 간단하다. 최종 아웃풋 배열을 Left 배열구성할 때와 동일한 방식으로 만든다.
그리고 Right 배열을 구현하지 않고 오른쪽에서 왼쪽으로 곱할 것을 곱해가면서 배열을 업데이트할 것이다. 그게 위의 최종 해답.


# 134. Gas Station

## Solution
```python
class Solution(object):
    def canCompleteCircuit(self, gas, cost):
        if sum(gas) < sum(cost):
            return -1
        start = 0
        tank = 0
        for i in range(len(gas)):
            tank += gas[i] - cost[i]
            if tank < 0:
                start = i + 1
                tank = 0
        return start

```

## Key Takeway
이 문제는 사실 답을 알고 있어야 푸는 문제에 가깝다. 정답이 그리디 방식으로 정확히 문제를 통찰해서
답을 구해야 하는데 그러기가 어렵기 때문.

일단 circular에 꽂혀서 무언가 modulo로 순환 처리를 해주어야 한다고 믿을 수 있지만 그럴 필요가 전혀 없고
일방향으로 index를 훑으면서 확인하기만 해도 된다는걸 캐치해야한다. 어떻게 캐치하게 되는지는 후술.

그리고 언제나 해법이 최소 하나는 있다는 말은 그 해법 상의 주행 중에는 가스가 음수가 되는 경우가 없다는 말이고,
이는 곧 전체 가스의 합이 주행 소모분을 제외해도 0 이상이라는 의미다. 

문제를 O(N)으로 해결하게 해주는 가장 핵심적인 통찰은, 매 인덱스마다 순이득/손실인 가스양을 계산해가면서
"가스 양이 가장 낮아지는 인덱스 바로 다음 인덱스"부터 시작하면 해법일 확률이 높아지고, 실제 해법도 그런 "다음 인덱스"들 중에
하나에 있다는 것을 깨닫는 것이다. 

그러고 나면 처음부터 쭉 순회하면서 가장 가스가 적어지는 시점을 계속 체크하면서 그런 지점을 발견하면 정답을 바로 그 다음 인덱스 자리로 업데이트해가며
one-pass로 O(N)만에 답을 찾을 수 있다. 이렇게 하면 되다보니 굳이 순회를 생각할 필요도 없다. 왜냐하면 내가 중간에 해법을 찾으면 끝까지 도달 후에 다시
거꾸로 처음부터 내가 찾은 해법 바로 전 자리까지 도달하기에 가스양이 충분하다는게 보장되기 때문. 따라서 가장 낮은 지점을 한 번 찾았으면 뒤돌아볼 필요가 없다.
해법은 무조건 그 다음 자리에 있을 것이기 때문. 

결국 요점은 이 순환 문제를 어떻게 one-pass에 최적지점을 찾기만 하면 되는 문제로 전환하는지에 있다.
전체 가스 양이 반드시 0보다 크다는 것,
해법이 반드시 존재하기 때문에 가스 잔량이 가장 낮아지는 시점만 찾으면 해법이 그 뒤에 반드시 존재한다는 것,
가스 잔량이 가장 낮아지는 시점을 한 번 찾으면 거꾸로 돌아오는건 고민 안해도 된다는 것
이 3가지를 정확히 캐치해내야 헤매지 않고 문제를 풀 수 있다.

코드 자체는 너무나 단순하다.




# 135. Candy

## Solution
```python
class Solution:
    def candy(self, ratings):
        candies = [1] * len(ratings)
        for i in range(1, len(ratings)):
            if ratings[i] > ratings[i - 1]:
                candies[i] = candies[i - 1] + 1
        sum = candies[-1]
        for i in range(len(ratings) - 2, -1, -1):
            if ratings[i] > ratings[i + 1]:
                candies[i] = max(candies[i], candies[i + 1] + 1)
            sum += candies[i]
        return sum
```

## Key Takeway

brute force로 할 경우 당연히 O(N^2)이 나온다. 왜냐하면 한 번에 하나의 index만 따진다고 해도
그 앞 뒤의 캔디 수를 계속 체크해서 캔디를 하나 업데이트할 때마다 확인을 해야하는데 캔디의 수도 n개라고 치면 n의 거듭제곱이 나온다.

그리고 단순히 비슷한 로직으로 one-pass를 시도해보려고 해도 깨닫게 되는 것이,
만약 index 0부터 len(ratings)-1까지 왼쪽에서 오른쪽으로 간다고 치면 rating 숫자가 4 3 2 1처럼 내림차순이면 문제가 된다.
왜냐하면 이미 지나온 부분도 업데이트해줘야 하는 상황이 나타나기 때문.
마찬가지로 이걸 뒤에서 시작하더라도 그러면 이번엔 오름차순인 1 2 3 4 이런게 문제가 된다.

기가막힌 one-pass해법을 찾지 못했더라도 이렇게 거듭제곱이 나타나는 이유가 단순히 방향 때문이라면,
배열 2개를 써서 이를 해결할 수도 있다. 하나의 배열은 왼쪽에서 오른쪽으로 갈때 더해지는 사탕만 따지고,
나머지 하나의 배열은 오른쪽에서 왼쪽으로 갈 때 더해지는 사탕만 따지는 것.
그리고 실제 부여하는 사탕수는 같은 위치에 대해 왼쪽배열과 오른쪽배열중 가장 큰 값을 할당하는 것이다.

예를 들어 12 4 3 11 34 34 1 67 이런 ratings가 있다면,
왼>오 배열은 1 1 1 2 3 1 1 2 이렇게 생길 것이고
오>왼 배열은 3 2 1 1 1 2 1 1 이렇게 생길 것이며
둘의 max값만 놓고보면 3 2 1 2 3 2 1 2 이렇게 될 것이고 결국 총합은 16이 될 것.

이러면 시간복잡도를 O(N) 수준으로 끌어내릴 수 있다. 

그리고 당연하지만 배열을 여러개 쓸 것도 없이 그냥 하나의 candies라는 배열을 만들어 업데이트 방향만 바꿔서 하면
배열을 하나로 유지할 수도 있다. 면접 현장에선 이정도만 해도 충분할 것.
