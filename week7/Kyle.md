
# 58. Length of Last Word

## Solution
```python
class Solution:
    def lengthOfLastWord(self, s):
        p, length = len(s), 0

        while p > 0:
            
            p -= 1

            if s[p] != " ":
                length += 1

            elif length > 0:
                return length

        return length
```

## Key Takeway
이 문제에서 실제로 서로 겹치지 않으면서 서로 다른 경우의 수는 세 가지 뿐이다:

The input string could be empty.

There could be some trailing spaces in the input string, e.g. hello <space>.

There might only be one word in the given string.
	
단순하게 생각해서 뒤에서부터 빈칸을 제거하며 오다가 빈칸이 아닌 글자를 처음 만나 순간부터
다음 빈칸을 만날 때까지 카운터를 하나씩 올리면서 오면 시간복잡도 O(N), 공간복잡도 O(1)으로 답을 낼 수 있다.
	
빈칸 제거를 단순히 while 룹으로 아래와 같이 구성할 수도 있고,
```python
    p = len(s) - 1
    while p >= 0 and s[p] == " ":
        p -= 1
```

(이러면 p에 빈칸이 아닌 최초의 글자가 있는 곳의 인덱스가 남는다)
그냥 trimming 함수를 쓸 수도 있다. 아래 예시는 rstrip()을 쓴 예:
```python

class Solution(object):
    def lengthOfLastWord(self, s):
        count = 0
        s = s.rstrip()
        for i in range(len(s) - 1, -1, -1):
            if s[i] == ' ':
                return count
            else:
                count += 1
        return count
```
근데 이걸 원패스로 할 수도 있다. 뒤에서부터 조사해오면서 빈칸이 "아니면" 카운터를 하나 올리고
"빈칸이면" 카운터가 양수일때만 반환하는 것이다.
```python
class Solution:
    def lengthOfLastWord(self, s):
        p, length = len(s), 0

        while p > 0:
            p -= 1
            # we're in the middle of the last word
            if s[p] != " ":
                length += 1
            # here is the end of last word
            elif length > 0:
                return length

        return length
```
이러면 복잡도는 똑같지만 순회는 한 번만 한다.  

빌트인 함수를 작정하고 쓰면 한 줄짜리 답안을 낼 수도 있다.
```python
class Solution:
    def lengthOfLastWord(self, s):
        return 0 if not s or s.isspace() else len(s.split()[-1])
```


# 14. Longest Common Prefix

## Solution
```python
class Solution(object):
    def longestCommonPrefix(self, strs):
        """
        :type strs: List[str]
        :rtype: str
        """
        if not strs or len(strs) == 0:
            return ""
        min_len = min(len(s) for s in strs)   
        if min_len == 0:                      
            return ""
        parts = []
        for i in range(min_len):
            for j in range(len(strs) - 1):
                if strs[j][i] != strs[j + 1][i]:
                    return "".join(parts)
            parts.append(strs[0][i])
        return "".join(parts)
```

## Key Takeway

이건 시간복잡도의 일반적인 상황에서의 최적화가 불가능한 문제다.
단순히 문자열 중 가장 짧은 문자열을 찾아서, 그 문자열을 기준으로 첫 글자부터 하나씩
문자열마다 돌아가며 조사를 하다가 틀린게 나오면 그때 멈추면 그게 최적 시간복잡도를 가질 것이다.
그러면 시간복잡도가  O(Nm^2)가 나올 것이다(m은 가장 짧은 문자열 길이).
				
이걸 다른 find()메서드같은걸 활용해서 작성해도 좋지만 위와 같이 구성하면 충분히 풀 수 있다.
다만 파이썬에서는 concatenation은 quadratic하다는 것을 잊지말고 + 대신 반드시 "".join()을 활용해주도록 하자.


# 151. Reverse Words in a String

ㄹ## Solution
```python

```

## Key Takeway


