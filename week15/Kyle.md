
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

```

## Key Takeway


