
# 125. Valid Palindrome

## Solution
```python
class Solution:
    def isPalindrome(self, s):

        i, j = 0, len(s) - 1

        while i < j:
            while i < j and not s[i].isalnum():
                i += 1
            while i < j and not s[j].isalnum():
                j -= 1

            if s[i].lower() != s[j].lower():
                return False

            i += 1
            j -= 1

        return True
```

## Key Takeway

이 문제는 정말 쉬운 문제다. 왜냐하면 문자열이 palindrome인지만 따지는 것이지
palindrome의 permutation(섞어놓은거)인지 따지는게 아니기 때문.

해야할 일은 빈칸 없애고 alphanumeric한 글자만 남기는 것.
하나씩 조사하면서 일치하는지 확인하는 것.

그래서 아래와 같이 답안 구성 가능:
```python
class Solution(object):
    def isPalindrome(self, s):
        s = ''.join([char for char in s if char.isalnum()])
        s = s.lower()
        for i in range(len(s)//2):
            if s[i] != s[len(s) - 1 - i]:
                return False
        return True
```

시간복잡도 O(N)은 못줄이므로 여기서 공간복잡도를 줄이고 싶으면, 포인터 두개로 잡고
alphanumeric인 것만 따져가며 비교하면 된다. 그 해답이 위 해답.


# 392. Is Subsequence

## Solution
```python
class Solution(object):
    def isSubsequence(self, s, t):
        pointer_s = 0
        pointer_t = 0
        count = len(s)
        while pointer_s < len(s) and pointer_t < len(t):
            if s[pointer_s] == t[pointer_t]:
                pointer_s += 1
                count -= 1
            pointer_t += 1

        return count == 0
```

## Key Takeway
이 문제는 은근히 어려운데,
특히 단순히 반복문 두 개로 하려고 할 때 그런데 순회는 한 번만 하고 싶을 때 예외들이 끝까지 발목을 잡는다.

단순히 생각하기에 바깥 순환에서 s의 글자 하나마다 돌아가면서
t에서 있는지 확인하면 될거라고 생각할 수 있는데,
s에만 있고 t에는 없는 글자가 중간에 있거나, 글자는 다 있는데 순서가 바뀌어있거나
한 경우를 전부 파악해서 해결하려면 조건문이 굉장히 지저분해지고 시간도 오래 걸린다.

그래서 그냥 투 포인터로 가는게 맞다.

이러면 시간복잡도는 O(T) 즉 t의 길이만큼만 나오고 공간복잡도는 O(1)이 나온다. 따로 뭘 더 만들어 출력하지 않기 때문.


# 167. Two Sum II - Input Array Is Sorted

## Solution
```python

```

## Key Takeway
