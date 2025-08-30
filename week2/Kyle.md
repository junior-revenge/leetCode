
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
        n = len(nums)
        k %= n

        start = count = 0
        while count < n:
            current_index, value = start, nums[start]
            while True:
                next_index = (current_index + k ) % n
                nums[next_index], value = value, nums[next_index]
                current_index = next_index
                count += 1
                if start == current_index:
                    break
            start += 1
```

## Key Takeway
pythonic한 방법으로 풀기 위해서는, slicing을 활용하면 근사하지만, 시간복잡도 측면에서 brute force 방식에 비해 나은 게 없다. 결국 배열의 모든 요소를 순회하며 O(kN)의 시간복잡도를 갖기 때문. 따라서 완전히 새로운 방법으로 접근해야 최적화가 가능하다.

이를 위해 회전이라는 개념을 순열과 사이클로 이해해본다. 결론부터 말하면, 요소를 다음 위치로 이동시키고, 또 그 자리에 있다가 비켜난 두 번째 요소를 그 요소가 가야할 또다른 자리로 이동시키는 일종의 "돌려막기"를 개념화한 것이다.

모든 회전(rotation)은 곧 순열(permutation)이다. 그말은 요소가 회전할 시 기존 위치가 새로운 위치로 매핑될 때 일대일 대응이 이루어진다는 의미이다. 그리고 순열은 언제나 서로소(disjoint)인 여러개의 순환(cycle)으로 분해된다. n개의 요소로 이루어진 배열을 k자리만큼 회전한다고 했을 때, n과 k의 최대공약수(Greatest Common Divisor, GCD)는 곧 그 순열을 분해한 사이클의 수를 나타낸다. 이 때, 각 사이클의 길이는 n / gcd(n,k)이다. 

사이클을 곧 배열 길이 n, 오른쪽으로 k칸 회전하는 인덱스에 대한 함수로 표현하면 다음과 같다:
f(i) = (i + k) % n
즉 인덱스 i에 있던 값은 회전의 결과로 f(i)의 위치로 이동한다는 뜻이다. 이때 시작점을 하나 정하고 그 시작점에서 회전을 시작해 그 시작점으로 다시 돌아올 때 까지 방문한 인덱스들의 집합이 사이클이다. 사이클 길이는 곧 이 인덱스들의 갯수를 의미한다.

예를 들어 n이 6, k가 4라면, 인덱스는 0부터 5까지일 것이다. 0을 시작점으로 잡으면 위 함수에 따라 0은 4로, 4의 값은 다시 2로, 2의 값은 다시 0으로 돌아오게 되며 이렇게 하나의 사이클이 종료가 된다. 이 때 사이클의 길이는 0, 4, 2의 3이다. 이제 아예 방문하지 못했던 인덱스 1을 시작점으로 새로운 사이클을 시작한다. 그러면 1 -> 5 -> 3 -> 1이라는 과정을 거치므로 (1, 5, 3)으로 구성된 두 번째 사이클을 구할 수 있다. 이렇게 두 사이클을 구하면 결국 모든 인덱스를 거친게 된다. 이 사이클의 수 2는 n과 k의 최대공약수이고, 그 길이는 n인 6을 최대공약수인 2로 나눠준 수인 3과 같다.

따라서 우리는 하나의 반복문이 하나의 사이클을 돌도록 구성할 것이며, 그렇게 하면 배열의 모든 요소 n개를 각 사이클을 통해 한 번씩만 방문하며, 방문하여 처리할 때 그 요소를 바로 최종 위치로 가져다 놓으므로 시간 복잡도가 brute force의 O(kN)보다 빠른 O(N)이 나온다.

여러 개의 순환으로 구성된 순열을 순회할 때, 몇 번째 사이클을 순회중인지 파악하기 위해 count 변수로 그 횟수를 세어야 한다. 

또 매 사이클 마다 시작점인 start를 한 칸씩 옮겨주는 것도 중요하다. 그렇게 옮겨간 새로운 사이클 내에서 start 인덱스 위치의 값을 value로 따로 저장해두고, 다음 위치 인덱스를 next_index로 구한 뒤 그 위치의 값을 새로운 value로 저장하고 기존에 value에 저장되어있던 값을 그 next_index위치에 넣는다. 이렇게 매 순회마다 들고온 값을 넣고 있던 값은 꺼내서 새로 드는 식으로 회전한다.
 
