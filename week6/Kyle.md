
# 42. Trapping Rain Water

## Solution
```python
class Solution:
    def trap(self, height):
        """
        :type height: List[int]
        :rtype: int
        """
        left, right = 0, len(height) - 1
        ans = 0
        left_max, right_max = 0, 0
        while left < right:
            if height[left] < height[right]:
                left_max = max(left_max, height[left])
                ans += left_max - height[left]
                left += 1
            else:
                right_max = max(right_max, height[right])
                ans += right_max - height[right]
                right -= 1
        return ans
```

## Key Takeway
이 문제는 brute force부터 정확히 짚어내야 최적화 답변도 이해할 수 있다.

특정 위치 i를 기준으로 그 왼쪽에서 가장 큰 것과 그 오른쪽에서 가장 큰 것 둘 중
가장 작은 높이에서 위치i의 높이를 빼는 것이 그 위치에서의 물의 양이 된다. 이걸 모르고 고안해내기란 은근히 어렵다.

brute force는 당연히 시간복잡도가 O(N^2)가 나오므로, 메모이제이션 해법을 찾아야 한다.

brute force에서는 i위치가 바뀔때마다 계속 왼쪽 맥스와 오른쪽 맥스를 다시 계산하므로 반복이 발생한다. 따라서 이를 방지하는 방식으로
메모이제이션을 적용하면 될 것이다. 이를 위해 왼쪽 맥스값과 오른쪽 맥스값을 저장할 배열을 두 개 관리한다.

여기서 매 인덱스 i에 이전까지의 맥스값을 저장하는 방식에 주목하면 좋다.

이러면 for loop이 세 번 도는게 전부이므로 시간복잡도를 O(N)으로 낮출 수 있다.

이걸 원패스로 하려면 투포인터 기법을 쓰면 된다. 이러면 배열을 여러개 안 만들어도 되므로 공간복잡도도 O(1)으로 줄일 수 있다.
논리자체는 간단한데 이걸 노베이스로 생각하려면 쉽지 않다. 

왼쪽 포인터는 0에, 오른쪽 포인터는 마지막 위치에 두고,
while문을 돌리면서 왼쪽포인터 높이와 오른쪽 포인터 높이를 비교해서 왼쪽이 더 낮으면 그리고 왼쪽 max값보다 왼쪽 높이가 더 높으면 왼쪽 max를 업데이트하고, 
왼쪽 max값이 더 크면 업데이트를 따로 안 한다. 그리고선 정답에 왼쪽 max값에서 왼쪽 포인터의 높이를 뺀 값을 더한다. 
그리고 왼쪽 포인터를 한 칸 움직인다.
반대로 오른쪽 포인터에 대해서도 같은 작업을 해준다.

이게 생각하기 쉽지 않은데, 일단 포인터를 양 끝에 두고 하나씩 좁혀온다는 생각은 어렵지 않다.
그 다음 왼쪽 max값보다 왼쪽 포인터위치의 높이가 더 크지 않다면 왼쪽 max값을 그대로 둬야한다. 크다면 높이가 더 높아지는 거니 물이 고여있을리 없고
낮다면 내려가는 것이니 그만큼 물이 고인다고 가정한다. 그 다음 답변을 왼쪽 max에서 왼쪽 포인터위치의 높이를 뺀 값만큼 더해준다.
이러면 물이 고여있으면 양수가 더해지게 된다.

여기서 "어떻게 왼쪽에서 오른쪽으로 진행하면서 높이가 낮아지면 그게 반드시 물이 차는 거냐"고 의문을 제기할 수 있는데,
맨 처음에 왼쪽 포인터 위치의 높이와 오른쪽 포인터 위치의 높이를 비교해서 오른쪽 포인터의 높이가 더 클 때만 이걸 진행하므로,
이 문제가 해결된다. 이게 직관적으로 와닿지 않을 수 있는데, 그림을 그려보면, 전체 배열에 대해 단 한순간이라도 오른쪽 포인터 위치가
왼쪽 포인터보다 높은 경우가 생긴다면 그 왼쪽 포인터에서 아래로 내려갈때는 반드시 물이 가득  차게 된다.

0 2 0 1  이러면 왼쪽이 더 높으니 오른쪽 포인터의 높이값이 항상 낮아 2 -> 0으로 갈 때 물이 찬다고 할 수 없다.
0 2 0 3 이러면 오른쪽 포인터 높이값이 언제나 왼쪽 포인터의 높이값보다 높으니 2 -> 0 으로 갈 때 물이 반드시 찬다.

이 사실을 빨리 캐치해야 최적 해법을 투포인터로 찾을 수 있다.


# 13. Roman to Integer

## Solution
```python
values = {
    "I": 1,
    "V": 5,
    "X": 10,
    "L": 50,
    "C": 100,
    "D": 500,
    "M": 1000,
}


class Solution:
    def romanToInt(self, s):
        total = 0
        i = 0
        while i < len(s):
            # If this is the subtractive case.
            if i + 1 < len(s) and values[s[i]] < values[s[i + 1]]:
                total += values[s[i + 1]] - values[s[i]]
                i += 2
            # Else this is NOT the subtractive case.
            else:
                total += values[s[i]]
                i += 1
        return total

```

## Key Takeway
일단 로마자와 정수의 매핑을 기록해둔다.
values = {
    "I": 1,
    "V": 5,
    "X": 10,
    "L": 50,
    "C": 100,
    "D": 500,
    "M": 1000,
}

여기서 중요한 사실을 알아야 하는데, 각 로마자는 그 로마자보다 값이 작은게 그 앞에 오는 경우를 제외하고는
언제나 수를 더한다는 점이다. 
그래서 현재가 마지막 위치인지 확인하고( i + 1 < len(s) ), 아니라면 현재 위치의 값이 그 다음 위치의 값보다
작은지 판단해서 작을 때는 그 다음 위치의 값에서 현재 위치의 값을 뺀 값을 더해주고 "두 칸"을 건너가야 한다. 
그게 아니라면 그냥 현재 칸을 더해준다. 
이 값을 빼준다는게 생각보다 어렵게 느껴질 수 있는데, 로마자 특성상 개별 기호 왼쪽에 오는 자기보다 작은 수는
그 개별 기호에서 뺄셈을 하는 것이기 때문에 이게 맞다. IV면 4인 이유는 V에서 1(I)을 뺀 것이기 때문. VI는 더한 거고.
그래서 뺄셈 처리 후에는 두 칸을 건너뛰어주고, 그게 아니라면 그 개별 기호보다 큰 값은 이후에 나타날 일이 없으므로
그냥 더해주기만 하면 된다.

이렇게 만해도 시간복잡도 공간복잡도 모두 O(1)이 나온다(문제의 조건 하에서). 이걸 아주 조금 더 개선하려면 CM, CD 같은 두 글자 숫자들을 매핑에 더해줘서 7개가 아니라 최대 13개까지 매핑해서 매번 순회때마다 이게 두 글자짜리 기호인지 한 글자짜리 기호인지 판단해서 건너뛰게 할 수도 있는데 복잡도를 바꿀정돈 아니다.

# 12. Integer to Roman

## Solution
```python
class Solution:
    def intToRoman(self, num):
        digits = [
            (1000, "M"),
            (900, "CM"),
            (500, "D"),
            (400, "CD"),
            (100, "C"),
            (90, "XC"),
            (50, "L"),
            (40, "XL"),
            (10, "X"),
            (9, "IX"),
            (5, "V"),
            (4, "IV"),
            (1, "I"),
        ]

        roman_digits = []
        # Loop through each symbol.
        for value, symbol in digits:
            # We don't want to continue looping if we're done.
            if num == 0:
                break
            count, num = divmod(num, value)
            # Append "count" copies of "symbol" to roman_digits.
            roman_digits.append(symbol * count)
        return "".join(roman_digits)
```

## Key Takeway
이 문제의 경우 로마자를 정수로 바꾸는 것과 비슷하지만,
숫자를 로마자로 표기하는 방법을 미리 숙지해둬야 안 헷갈릴 수 있다.
예를 들어 140을 표시하는 방법은 CXXXX도 되고 CXL도 되고 CXXVVVV도 되지만,
여기서 가장 응축이 되는 CXL이 옳은 표기법이다. 

이건 되는 숫자부터 문자로 바꿔보는, 그리디 알고리듬 방식으로 접근하면 된다.
직관적으로 떠올리기 쉽다.

divmod()를 쓰면 자연스레 몫과 나머지로 된 튜플을 반환하니 유용하다.
몫은 몫 갯수만큼 기호를 반복해 붙이고 나머지는 그대로 새로운 num으로 활용한다.

