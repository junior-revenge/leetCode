
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
class Solution:

```

## Key Takeway





# 135. Candy

## Solution
```python


```

## Key Takeway
