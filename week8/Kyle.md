
# 6. Zigzag Conversion

## Solution
```python
class Solution:
    def convert(self, s, num_rows):
        if num_rows == 1:
            return s

        res = []

        for r in range(num_rows):
            increment = 2 * (num_rows - 1) 
            # You figure this out by checking sample cases. 
            # This doesn't go below 0 since we start with num_rows higher or equal than 2.
            for i in range(r, len(s), increment):
                res.append(s[i]) # This would work perfectly for the first and last row only
                # rows in between have extra value not captured by simply hopping by 'increment'
                # distance. So we take care of that as well.
                # For that, we check if the row is in the middle, and also if the extra 
                # characters in the middle are in bounds. They are in i + increment - 2*row
                # because every row you go down the distance decreases by 2*row
                if (r > 0 and r < num_rows - 1 and i + increment - 2 * r < len(s)):
                    res.append(s[i + increment - 2 * r])

        return "".join(res)
```

## Key Takeway
이 문제는 차근차근 시행착오를 겪으며 정확한 공식을 도출해내면 사전지식 없이도 풀 수 있는 문제다.
그러나 실제로는 그러고 있을 시간이 없으므로 몇 가지 핵심을 기억해내야 시간 내에 풀 수 있다.
실수로 매트릭스를 구성해서 숫자를 채워넣는다거나 하면 행과 열 개념으로 2차원으로 접근하려했다가 더 어렵게 풀 수도 있으니 주의.

일단 몇 개의 행으로 글자를 배열할건지에 따라서 글자 위치가 결정되므로,
1)num_rows만큼 가장 바깥 반복을 돌려야 한다는 것을 깨닫고
2)내부 반복문에서는 그거만큼 건너뛰면서 글자를 붙여나가야 함을 이해하고
3)맨 첫 줄과 맨 마지막줄을 제외하면 누락된 글자가 발생함을 이해하며,
4)그 누락된 글자를 정확한 조건을 찾아 추가해줄 수 있어야 한다.

이 4가지 과정 중 하나라도 놓치면 시간 내에 정답을 찾기 어려워지는 문제다.

시간복잡도는 O(num_rows * N) 즉 O(N)이 된다.

글자들이 가장 위쪽 줄(index 0)과 가장 아랫 줄(index len(s) - 1)사이를 튕긴다는 점에 착안하여 아래와 같은 해법으로
풀 수도 있다. 기본적인 아이디어는 맨 윗줄이나 맨 아랫줄에 부딪힐때마다 인덱스를 늘리거나 줄이는데 그게 부딪힐때마다 방향이 바뀌도록 설정해준 것.
다만 concatenation을 안쓰려고 하다보니 조금 복잡한 문법이 되었기도 하다. 
```python
class Solution:
    def convert(self, s, numRows):
        if numRows == 1:
            return s

        rows = [[] for _ in range(numRows)]
        going_up = True
        index = 0

        for char in s:
            rows[index].append(char)
            if index == 0 or index == numRows - 1:
                going_up = not going_up
            index = index - 1 if going_up else index + 1

        return "".join("".join(r) for r in rows)
```

# 28. Find the Index of the First Occurrence in a String

## Solution
```python
class Solution(object):
    def strStr(self, haystack, needle):
        if len(needle) == 0:
            return 0 
        complete = True
        i = 0
        while i < len(haystack) - len(needle) + 1:
            for x in range(len(needle)):
                complete = True
                if haystack[i + x] != needle[x]:
                    complete = False
                    break
            if complete:
                return i
            i += 1
        return -1
```

## Key Takeway
brute force로 풀어보자.
일단 haystack의 각 인덱스에 대해서, 
needle의 각 글자를 하나씩 대조해보고,
만약 중간에 하나라도 일치하지 않을 경우, 그 인덱스에 대해서는 작업이 끝났다고 보고 인덱스를 1 증가시켜 동일한 작업을 반복해야 하고,
전부 대조해도 동일하다면 작업을 중단하고 그 위치에서의 인덱스를 반환해야 할 것이다.
반복문 두 개를 사용하면 쉽게 가능하다.



시간복잡도는 O(MN)으로, 더 최적화할 필요는 없다.
				
파이썬 슬라이스를 쓰고 싶으면 아래와 같이 하면 된다:
```python
if haystack[i:i+len(needle)] == needle: return i
```

# 68. Text Justification

## Solution
```python



```

## Key Takeway




