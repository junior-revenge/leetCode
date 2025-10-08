
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

## Key Takeway

# 167. Two Sum II - Input Array Is Sorted

## Solution
```python

```

## Key Takeway
