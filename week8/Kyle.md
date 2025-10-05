
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

class Solution(object):
    def fullJustify(self, words, maxWidth):
        output = []
        end = False
        n = len(words)
        i = 0  # index into words

        while i < n:
            words_to_justify = []
            count = 0
            # pack from i forward (same logic, just using index)
            for k in range(i, n):
                if len(words[k]) <= maxWidth and len(words[k]) <= maxWidth - count:
                    count += len(words[k])
                    words_to_justify.append(words[k])
                    if count + 1 <= maxWidth:
                        count += 1
                else:
                    break

            taken = len(words_to_justify)
            i += taken  # advance instead of del words[:taken]

            # mark last row if no words remain
            if i == n:
                end = True

            # === build justified line (ARITHMETIC, no regex) ===
            if end or len(words_to_justify) == 1:
                # last line or single word -> left-justify
                justified = ' '.join(words_to_justify)
                while len(justified) < maxWidth:
                    justified += ' '
            else:
                # fully-justify by distributing spaces once
                words_len = sum(len(w) for w in words_to_justify)
                spaces = maxWidth - words_len
                gaps = len(words_to_justify) - 1
                q, r = divmod(spaces, gaps)  # base and extras

                parts = []
                for idx, w in enumerate(words_to_justify[:-1]):
                    parts.append(w)
                    # at least 1 space between words + q base + 1 for first r gaps
                    parts.append(' ' * (q + (1 if idx < r else 0)))
                parts.append(words_to_justify[-1])

                justified = ''.join(parts)

            output.append(justified)

        return output


```

## Key Takeway

문제가 복잡해보이지만 막상 할 일이 많아서 그렇지 brute force로 푼다면 그렇게 어렵기까지만한 문제는 아니다.

하나의 행에 들어갈 단어들을 그리디하게 추려내고,
그 단어들 사이에 전체 조건에 맞게 빈칸을 집어넣어주고
단어가 하나뿐인 경우와 마지막 행에 대한 특별한 처리를 해주면 된다.

아래는 성공한 시도:

```python
import re

class Solution(object):
    def fullJustify(self, words, maxWidth):
        output = []
        end = False
        while len(words) > 0:
            words_to_justify = []
            count = 0
            for i in range(len(words)):
                if len(words[i]) <= maxWidth and len(words[i]) <= maxWidth - count:
                    count += len(words[i])
                    words_to_justify.append(words[i])
                    if count + 1 <= maxWidth:
                        count += 1
                else:
                    break
            del words[:len(words_to_justify)]

            # mark last row if no words remain
            if len(words) == 0:
                end = True
                
            ## you justify
            justified = ' '.join(words_to_justify)

            if not end:
                remaining_space = maxWidth - len(justified)
                while remaining_space > 0:
                    # find a space after a word and add space
                    gaps = [m for m in re.finditer(r' +', justified)]
                    if gaps:
                        # add to the leftmost gap with the fewest spaces
                        min_len = min(len(m.group(0)) for m in gaps)
                        for m in gaps:
                            if len(m.group(0)) == min_len:
                                insert_at = m.end()
                                justified = justified[:insert_at] + ' ' + justified[insert_at:]
                                break
                    else:
                        # if there's only one word, pad at the end
                        while len(justified) < maxWidth: 
                            justified += ' '
                        break
                    remaining_space -= 1
            else:
                while len(justified) < maxWidth: 
                            justified += ' '
            output.append(justified)

        return output
```
여기서 비효율적인 부분을 하나씩 개선해나가면 충분한 답이 된다.
일단 del words[:len(words_to_justify)] 이거는 삭제를 할 때마다 요소를 전부 끌어오는
O(N)의 시간복잡도를 가진 작업이므로 인덱스를 하나 추가해 활용해서 이를 방지한다.
새 코드에서는 while문 안에서 i를 활용하고 한 행에 넣을 단어들의 길이를 잰 뒤 taken에 저장하고
그 taken만큼 i를 늘린다. 

그리고 정규식을 사용하기 보다는 divmod()를 활용해 한 번에 넣을 빈칸을 계산해서 넣어준다.
어려울 것 같아도 아주 쉬운데, 일단 글자의 수를 다 더해서 빈칸이 몇개가 들어가야하는지 샌 뒤,
단어들의 갯수 - 1을 해서 들어가야할 gap의 수를 구한다.
그리고 빈칸의 갯수를 gap의 수로 나눠주고 몫을 q로 나머지는 r로 저장한다. 
그리고 아래와 같이 붙인다:
					parts.append(' ' * (q + (1 if idx < r else 0)))
여기서 괄호 내부에선 공통적으로 q만큼 들어가야 하니 그렇게 넣고, 그러고도 남은 나머지 빈칸은 빈칸을 넣는 중인 그 단어의 인덱스가 나머지보다 작을 경우에 하나씩 넣어준다.
이게 헷갈릴 수 있는데, 예를 들어 단어가 3개있고 빈칸이 5개 들어가야하면, 몫 1 나머지 2니까,
인덱스 0 단어 뒤에 일단 몫(1)만큼 더하고 그리고 나머지가 2인데 인덱스가 0이니까 아직 인덱스가 작으니 나머지(1)만큼 또 더해준다. 인덱스가 1일때도 그렇게하고 인덱스가 2일 때는 나머지 1을 안 더해서 1만 더해지게 된다. 나머지와 인덱스를 활용해 자연스레 앞에서부터 남은걸 채워넣게 한 것.





