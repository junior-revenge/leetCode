
# 289. Game Of Life

## Solution
```python
class Solution:
    def gameOfLife(self, board):

        # Neighbors
        neighbors = [(1,0), (1,-1), (0,-1), (-1,-1), (-1,0), (-1,1), (0,1), (1,1)]

        rows = len(board)
        cols = len(board[0])

        for row in range(rows):
            for col in range(cols):

                # For each cell count the number of live neighbors.
                live_neighbors = 0
                for neighbor in neighbors:

                    # row and column of the neighboring cell
                    r = (row + neighbor[0])
                    c = (col + neighbor[1])

                    # Check the validity of the neighboring cell and if it was originally a live cell.
                    if (r < rows and r >= 0) and (c < cols and c >= 0) and abs(board[r][c]) == 1:
                        live_neighbors += 1

                # Rule 1 or Rule 3
                if board[row][col] == 1 and (live_neighbors < 2 or live_neighbors > 3):
                    # -1 signifies the cell is now dead but originally was live.
                    board[row][col] = -1
                # Rule 4
                if board[row][col] == 0 and live_neighbors == 3:
                    # 2 signifies the cell is now live but was originally dead.
                    board[row][col] = 2

        for row in range(rows):
            for col in range(cols):
                if board[row][col] > 0:
                    board[row][col] = 1
                else:
                    board[row][col] = 0
```

## Key Takeway
in-place로 바꾸는 문제인데 당연히 board 복사본을 운영하면 너무 쉽고
공간복잡도 O(1)으로 풀어낼 수 있어야 한다.

일종의 코딩기법인데, 원래부터 0이었던 것, 원래부터 1이었던것과 달리 
원래 0이었는데 1이된 것과 원래 1이었는데 0이 된 것을 별도의 방법으로 나타내면 된다.

예를 들어 1이었다가 0된거는 -1, 0이었다가 1된 거는 2라고 코딩하는 것이다. 

# 383. Ransom Note

## Solution
```python
class Solution:
    def canConstruct(self, ransomNote, magazine):
        mmap = defaultdict(int)

        for char in magazine:
            mmap[char] += 1

        for char in ransomNote:
            mmap[char] -= 1
            if mmap[char] < 0:
                return False

        return True
```

## Key Takeway
아주 심플한 문제.
처음엔 두 문자열 ransomnote와 magazine에 대해 각각 해시맵을 고안하고
서로 비교하겠다고 생각했지만, 굳이 그럴것도 없이 해시맵 하나에 매거진을 때려넣고
ransomnote의 글자들을 하나씩 빼다가 더 뺄 수 없으면 False반환하면 된다.

# 205. Isomorphic Strings

## Solution
```python
class Solution:
    def isIsomorphic(self, s, t):

        mapping_s_t = {}
        mapping_t_s = {}

        for c1, c2 in zip(s, t):

            # Case 1: No mapping exists in either of the dictionaries
            if (c1 not in mapping_s_t) and (c2 not in mapping_t_s):
                mapping_s_t[c1] = c2
                mapping_t_s[c2] = c1

            # Case 2: Either mapping doesn't exist in one of the dictionaries or Mapping exists and
            # it doesn't match in either of the dictionaries or both
            elif mapping_s_t.get(c1) != c2 or mapping_t_s.get(c2) != c1:
                return False

        return True
```

## Key Takeway

처음엔 글자별로 갯수세서 그 숫자들 패턴만 맞추면 되리라 생각했는데,
공간적인 관계정보도 보존해야한다.
s = "bbbaaaba"
t = "aaabbbba"

이 둘은 isomorphic하지 않기 때문. 특별히 대단한 해법은 없고, 결국 글자간의 매핑을 하나하나 경우를 나눠 따져봐야 한다.
그래서 s의 글자들이 t로 매핑되는 경우와 t의 글자들이 s로 매핑되는 경우를 비교해야하기 때문에 맵을 두개 활용한다.
두 문자열의 길이가 같다고 가정하기 때문에, 각 글자를 하나씩 같이 비교하는데
1)만약 둘 다 각자의 매핑에 추가된적이 없으면 서로를 교환해서 추가한다. 즉 s의 첫글자가 a고 t의 첫글자가 b면
a를 b로 그리고 b를 a로 매핑한다.

2)만약 두 글자중 한쪽만 매핑이 되어있거나 둘 다 매핑이 되어있는데, 그게 지금 같이 비교하는 두 글자 사이의 매핑이 아니라
다른 글자와의 매핑이면 false를 반환해버린다. 즉 a와 b 그리고 b와 a를 과거에 매핑해놨는데 다시 만난 a가 이번엔 b가 아니라 c로 매핑되어있으면 False 날리는 것.

이 경우를 다 통과하면 True를 반환해준다.  isomorphic에 대한 개념을 이해하는데 시간을 쓰면 easy치곤 푸는데 고심할 수 있는 문제.
