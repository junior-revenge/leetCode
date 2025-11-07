
# 30. Substring with Concatenation of All Words

## Solution
```python
from collections import defaultdict

class Solution(object):
    def findSubstring(self, s, words):
        output = []
        word_len = len(words[0])
        total_len = word_len * len(words)

        if len(s) < total_len:
            return []

        word_map = defaultdict(int)
        for word in words:
            word_map[word] += 1

        # try each offset
        for i in range(word_len):
            left = i
            right = i

            window_map = defaultdict(int)
            matched = 0

            while right + word_len <= len(s):
                w = s[right:right + word_len]
                right += word_len

                if w in word_map:
                    window_map[w] += 1
                    if window_map[w] <= word_map[w]:
                        matched += 1
                    else:
                        # too many of w → shrink from left by word_len
                        while window_map[w] > word_map[w]:
                            lw = s[left:left + word_len]
                            window_map[lw] -= 1
                            if window_map[lw] < word_map[lw]:
                                matched -= 1
                            left += word_len

                    if matched == len(words):
                        output.append(left)
                        # pop one from left to continue searching
                        lw = s[left:left + word_len]
                        window_map[lw] -= 1
                        matched -= 1
                        left += word_len
                else:
                    # reset window at next aligned position
                    window_map.clear()
                    matched = 0
                    left = right

        return output
```

## Key Takeway
처음 이 문제를 접하고 혼자 풀 때는,
슬라이딩 윈도우를 만들어서 words를 단어별로 매핑한 해시맵이랑 슬라이딩 윈도우 내의 문자들을 매핑한 해시맵을 비교해서 맞으면 넣고 아니면 한칸씩 이동하는 식으로 로직을 짰다. 이게 가능했던 이유는 단어들의 길이가 모두 같았기 때문. 
```python
class Solution(object):
    def findSubstring(self, s, words):
        output = []
        word_len = len(words[0])
        word_map = defaultdict(int)
        for word in words:
            word_map[word] += 1

        left = 0
        while left + word_len * len(words) - 1 < len(s):
            window_map = defaultdict(int)
            for i in range(left, left + word_len * len(words), word_len):
                window_map[s[i:i + word_len]] += 1

            if dict(window_map) == dict(word_map):
                output.append(left)

            left += 1

        return output
```				 
이러면 제출은 되고, 시간복잡도는 O(M * N * K) 즉 개별 단어의 길이, words의 길이, 전체 s의 길이를 곱한 값이 된다. 그러나 시간복잡도가 O(MNK)가 나오면 안되는 것이, 진짜 슬라이딩 윈도우라면 그 윈도우가 지나가는 배열은 시간복잡도 O(N)에 해결해야하는데 여기선 그러지 못하고 있다.
왜냐하면 left가 업데이트 될 때마다 새로 해시맵을 만들기 위해 윈도우를 다시 처음부터 탐색하는데, 이러면 당연히 중복이 생기기 때문. 이건 일종의 고정된 윈도우로 계속 다시 스캔하는게 되어서 '슬라이딩 윈도우'의 본질인 '슬라이딩'이 일어나지않는다. 

이걸 슬라이딩시키려면, left와 right으로 포인터가 나뉘어져 있을 때, 주어진 left에 대해 right 포인터는 오른쪽으로 진행하면서 찾고있던 단어가 다 나와서 permutation을 하나 찾았어도 계속 진행해서 동일한 단어가 다시 나올때까지 가야하고, 동일한 단어가 나오면 그제서야 left를 업데이트해주는 식으로 가야한다. 이래야만 전체 s를 한 번만 조사할 수 있고, 시간복잡도를 줄일 수 있다.
	
그리고 이걸 하려면 처음 words에 대해 만든 맵에서 갯수를 자꾸 업데이트해주는 식으로 관리해야한다.

그래서 최적답변을 보면, while right + word_len <= len(s): 이 블럭에서부터 right포인터를 쭉 끝까지 진행시킴을
알 수 있고 그렇게 새로 단어 w를 뽑아놓고 right 포인터는 바로 업데이트해준다. 
그리곤 이 w가 word_map에 있으면 해당 슬라이딩 윈도우의 맵인 window_map에서도 카운트를 올려주고 
window_map에 이렇게 카운트 올린게 word_map과 대치되지 않으면 (즉 word_map의 카운트를 넘지 않으면) matched를 하나 올린다.
그런데 이때 만약 대치되면, 즉 넘어서면, 그 때는 왼쪽 포인터를 움직일 때라는 뜻이므로 그게 다시 대치되지 않을 때까지 
왼쪽 단어를 lw로 뽑고 window_map에서 그 카운트를 하나 빼고 다시 확인해서 word_map 범위 안에 들어오면 matched도 하나 뺀다. 
그리고 이걸 내부 while룹을 돌려서 필요한만큼 left를 끌고 온다. 이렇게 오른쪽 업데이트와 필요에 따라 왼쪽 끌고오기를 해준 뒤
그렇게 매치된게 words의 길이와 일치하다면 그때는 결과를 저장한다. 
결과를 저장하고나서도 왼쪽에서 한 단어는 빼야하므로 lw를 똑같은 방식으로 지정해 window_map에서 하나 빼주고 matched도 하나 줄여준뒤 
다시 이 때도 left를 한칸 끌고와준다.
위 모든 과정은 처음 right포인터가 나서서 조사한 단어가 word_map에 있을 때고, 없다면 우리는 이 window_map을 버리고 matched를 0으로 세팅한뒤 
왼쪽 포인터도 오른쪽 포인터위치까지 끌고 온 다음에 전체를 다시 반복한다. 즉 w를 새로 뽑는 것.
	

# 76. Minimum Window Substring

## Solution
```python
class Solution(object):
    def minWindow(self, s, t):
        output = ""
        min_length = float('inf')
        t_map = defaultdict(int)

        for char in t:
            t_map[char] += 1

        left = 0
        right = 0
        window_map = defaultdict(int)
        matched = 0
        while right < len(s):
            
            if s[right] in t_map:
                window_map[s[right]] += 1
                if window_map[s[right]] == t_map[s[right]]:
                    matched += 1

                while matched == len(t_map):
                    if right - left + 1 < min_length:
                        min_length = right - left + 1
                        output = s[left:right + 1]
                    
                    left_ch = s[left]
                    if left_ch in t_map:
                        window_map[left_ch] -= 1
                        if window_map[left_ch] < t_map[left_ch]:
                            matched -= 1
                    left += 1
                    
            right += 1

        return output

```

## Key Takeway
잘 고민해보면 로직 세우는건 어렵지 않다. 혼자서 시행착오를 겪어보면서 아래와 같은 로직을 세우면 된다.

먼저 t를 전부 글자단위로 쪼개서 해시맵에 넣는다.

슬라이딩 윈도우의 두 포인터 left와 right을 함께 0에서 시작한다. 
슬라이딩 윈도우의 문자들을 저장하고 관리할 해시맵도 하나 만든다.
그리고 목표하는 글자의 갯수에 도달하면 그렇게 도달한 갯수를 저장관리하기 위한
변수 matched도 만들어 0으로 초기화한다.

오른쪽 포인터 right이 len(s)에 도달하기전까지 반복하면서
만약 해시맵에 현재 right포인터가 가리키는 단어가 있으면, 
슬라이딩 윈도우의 해시맵에 그걸 1 올리고 그러고나서 만약 두 해시맵의 값이 같으면
해당 문자에 대해선 매칭이 완료된 것이므로 matched를 1 올린다.

그렇게 매칭하고나서 matched값이 t해시맵에 필요한 모든 글자수와 일치하게 되면 드디어 필요한
글자가 윈도우 안에 다 들어온게 되므로 이때부터 왼쪽 포인터를 수축시키려고 시도한다.

matched값이 len(t_map)과 일치하는 동안 반복해서 만약 현재 슬라이딩 윈도우의 길이(right-left+1)이 역대 최소길이보다 작은지
확인해서 작다면 이 길이로 업데이트하고 반환할 output도 이 문자열로 업데이트해준다.

그러고나서 left를 하나 줄여본다. 이 때 가르키던 값이 t_map에 있는 유효한 글자였냐 아니냐가 중요한 부분이라
줄이기 전에 일단 s[left]값을 저장해둔 뒤 윈도우 해시맵에서 이 문자의 값을 1 빼고, 그렇게 해서 t_map의 해당 문자의 값(갯수)보다 작아지게 되면
matched 숫자도 1 줄인다. 

위 작업은 matched가 len(t_map)과 일치하는한 반복한다. 즉 유효하지 않은 문자는 왼쪽에서 계속 줄여오지만 유효한 숫자가 나와서 matched 숫자가 변하게 되면
멈추겠다는 뜻.

이 전체 과정을 right을 하나 옮길때마다 해준다. 

쉽게말해 right 포인터를 늘려가다가 우리가 필요한 문자열이 다 잡히면 그 단계 내에서 left포인터를 앞으로 끌고오고, 그러다가 left를 줄여서
유효한 문자가 윈도우를 벗어나게 되면 다시 right포인터가 오른쪽으로 늘어나는 식으로 작동한다.


이러면 두 문자열의 길이만큼만 순회가 이루어지므로 O(S+T) 의 시간복잡도가 나온다(여기서 S와 T는 s와 t의 길이).
공간복잡도 역시 마찬가지로 O(S+T)다. 윈도우 길이가 s만큼 길어지고 또 t에 포함된 문자가 모두 고유값일 경우의 최악에 S+T만큼 공간이 필요하기 때문.




# 36. Valid Sudoku

## Solution
```python
class Solution(object):
    def isValidSudoku(self, board):
        cols = defaultdict(set)
        rows = defaultdict(set)
        squares = defaultdict(set) # key = (row / 3 , column / 3)
        
        for r in range(9):
            for c in range(9):
                if board[r][c] == ".":
                    continue
                if (board[r][c] in rows[r] or
                    board[r][c] in cols[c] or 
                    board[r][c] in squares[(r // 3, c // 3)]): # board[r][c]값이 r행이나 c열이나 그 사각형에있는지 확인
                    return False
                cols[c].add(board[r][c])
                rows[r].add(board[r][c])
                squares[(r // 3, c // 3)].add(board[r][c])
        return True
```

## Key Takeway

헷갈리지 말아야 할 것은, 이미 채워진 숫자만 고려하는 것이라서 이 전체가 정답이 있는 스도쿠 문제인지는 중요하지 않고
현재 적혀있는 값중에 오류가 있는지만 파악해서 정답을 내면 된다.

기본적으로 brute force가 가능한 문제다.
그래서 그런식으로 생각의 밑그림을 깔아놓고
1번과 2번과 3번을 따로 조사한 뒤 결과를 합쳐 결론을 도출하면 된다. 

매 행과 열을 하나씩 조사할 때는 hash set을 활용해 중복값이 있는지 조사하면 되겠다. (defaultdict(set)을 활용해 hashset 만드는 방법 익혀두기)
이렇게 하면 hash set 하나 당 시간복잡도는 한 면이 N개의 숫자가 있다면 O(N^2)일 것이다.

부분의 구조가 반복되는 방식이므로 DP를 떠올릴 수 있는데, 일단 여기서는 그게 무한히 재귀되지 않고,
사각형 크기가 정해져있고 조사할 범위도 정해져 있다. 9X9 사각형 전체를 한 번 조사하고
그 다음에 3X3만 9개 더 조사하면 끝이 난다. 행과 열 조사할때도 행만 쭉 조사하는거랑 열만 쭉 조사하는거랑 어떤 상관성이 (설령 있더라도) 없다. 그냥 선형적으로 3 가지 사항을 조사하면 끝인 것. (행이랑 열이랑 서로 숫자가 중복되면 어떡하지 같은 고민 안해도 되는 것. 행 안에서 열 안에서 각각 중복이 없기만 하면 노상관)

위에서 행과 열을 O(9^2)로 두 개의 hash setㅇ르 활용해 조사했고 이제 9개의 3X3 미니 사각형들을 또 별도의 hash set을 하나씩 할당해서
조사해야 한다. (3X3은 행이나 열 단위로 조사하는게 아님을 주의할 것) 그럼 여기서도 시간복잡도는 9칸씩 가진 미니사각형
9개를 조사하는 것이니 O(9^2)가 또 나온다.

공간복잡도 역시 9^2 크기의 hash set을 3개 유지할거라 그냥 O(9^2)가 나온다.

