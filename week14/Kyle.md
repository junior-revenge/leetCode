
# 54. Spiral Matrix

## Solution
```python
class Solution:
    def spiralOrder(self, matrix):
        result = []
        rows, columns = len(matrix), len(matrix[0])
        up = left = 0
        right = columns - 1
        down = rows - 1

        while len(result) < rows * columns:
            # Traverse from left to right.
            for col in range(left, right + 1):
                result.append(matrix[up][col])

            # Traverse downwards.
            for row in range(up + 1, down + 1):
                result.append(matrix[row][right])

            # Make sure we are now on a different row.
            if up != down:
                # Traverse from right to left.
                for col in range(right - 1, left - 1, -1):
                    result.append(matrix[down][col])

            # Make sure we are now on a different column.
            if left != right:
                # Traverse upwards.
                for row in range(down - 1, up, -1):
                    result.append(matrix[row][left])

            left += 1
            right -= 1
            up += 1
            down -= 1

        return result
```

## Key Takeway
이 문제를 풀려고 하다보면 우선 하나하나 케이스를 따져보고, 반복되는 패턴이 있으니 재귀적으로 풀 수 있지 않을까 혹은 DP로 접근할 수 있지 않을까 착각할 수 있다.

그러나 실제로 코드를 짜다보면, 반복은 왼->오, 위->아래, 오->왼, 아래->위가 점점 더 안쪽 사각형으로 들어가면서 반복은 되는데, 그 반복 대상이 되는 실제 matrix의 값들이 계속 (체계적이긴 하지만) 달라진다.

그래서 바깥을 먼저 처리하고 바깥 한줄씩 벗겨낸 안쪽 matrix에 대해서 똑같은 연산을 하라고 할 수가 없다. 왜냐면 그 안에 들어가있는 값을 matrix에서 참조해야하는데 그러면 그때마다 matrix를 또 복잡한 방식으로 껍질(최와곽 값들)을 잘라서 전달해야하는데 그것부터가 엄청 실수하기 좋기 때문. 

문제를 풀어보려고 아래와 같이 다가갈 순 있는데, 이러면 저 인덱스 하나하나 설정하느라 머리가 터진다.

m = matrix.length
n = matrix[0].length

first round. 
matrix[0][0] matrix[0][1] ... matrix[0][n-1]    n개
matrix[1][n-1] matrix[2][n-1] ... matrix[m-1][n-1]   m - 1개
matrix[m-1][n-2] matrix[m-1][n-3] ... matrix[m-1][0] n - 1개
matrix[m-2][0] matrix[m-3][0] ... matrix[1][0] m - 2개

second round
matrix[1][1] matrix[1][2] ... matrix[1][n-2] n-1개
matrix[2][n-2] matrix[3][n-2] ... matrix[m-2][n-2] m -3개
matrix[m-2][n-3] matrix[m-2][n-4] ... matrix[m-2][1]  n-3개
matrix[m-3][1] matrix[m-4][1] ... matrix[2][1]

...

따라서 첫 4줄 (왼->오, 위->아래, 오->왼, 아래->위) 정도만 빠르게 해보고, 전체 순회를 while문으로 묶은 뒤 조건은 매트릭스 안에 들어있는 값들의 길이만큼 결과배열이 채워질때까지로 하고, 4가지 이동(왼->오, 위->아래, 오->왼, 아래->위) 에 대해 별도의 로직(4개의 for문)으로 이어붙이게 한 뒤, 그 때 이어붙일때의 바운더리, 즉 경계값을 매 while문마다 업데이트해주는 방식으로 접근하는 것이 가장 가독성이 좋다.
(이런 문제는 당연히 특별한 최적화가 불가능하기에 시간복잡도나 공간복잡도는 다른 접근을 활용하더라도 O(행X열) 그리고 O(1)이다.)

즉 이 문제는, 사실 대단한 문제가 아니다. 그냥 하나의 선형적인 배열을 둘둘 말아놨을 뿐이라서, 결국 어떤 선형 배열을 index 0부터 끝까지 딱 한번 순회하면 끝인데, 다만 중간중간 거기에 접근하는 인덱스가 차원이 하나 추가되어 좀 더 복잡해질 뿐, 그냥 배열 하나 순회해서 그 값을 저장한다 뿐이다.

특히 처음에 '왼'과 '위'를 둘 다 0으로 놓고, '오'와 '아래'를 각각 열의 끝, 행의 끝으로 놓으면서 이 해법의 간결함이 드러난다. 4개의 for문에서는 왼->오 이동을 left에서 right까지 그러나 붙이는건 matrix[첫행][iterator] 이런 식으로 붙인다. 마찬가지로 위->아래 이동을 위해서는 이번엔 위+1(1 더한게 디테일)에서 아래까지 순회하면서 matrix[iterator][마지막열]를 붙인다. 그리고 밑바닥의 오->왼 이동에서는 반대로 '마지막 열 - 1'에서 시작해서 하나씩 내려오면서 두번째열(이게 위에 적어놓은 예시와 좀 달라지는 부분)까지 내려오며 matrix[마지막행][iterator]를 더하고, 마지막으로 아래->위 이동에서는 맨 마지막에서 두번째 행부터 두번째 행까지 1씩 거꾸로 타고올라오면서 matrix[iterator][첫열] 값들을 더해온다. 이 때, 오->왼과 아래->위에서는 if문을 걸어줘야 하는데, 오->왼에서는 만약 up과 down이 만나버리면 더 진행하면 안되고, 아래->위에서는 이미 좌우가 만나버리면 진행하면 안되므로 if문을 걸어 그것을 방지하고 있다.

이렇게 한바퀴 돌고나서 왼쪽 첫 열과 맨 윗 행은 1씩 값이 증가하고, 맨 마지막 열과 맨 마지막 행은 1씩 값을 감소시킨다. 그리고 똑같은 작업을 반복하면 결국 원하는 값을 얻을 수 있게 된다.

아이디어는 안어려워서 한 번 해보면 괜찮은데, 처음에 좀 당황스러울 수 있는 문제.

	

# 48. Rotate Image

## Solution
```python
class Solution(object):
    def rotate(self, matrix):
        left = 0
        top = 0
        right = len(matrix) - 1 
        bottom = len(matrix) - 1 

        while left < right and top < bottom:

            for i in range(right - left):
                (
                    matrix[top + i][right], 
                    matrix[bottom][right - i], 
                    matrix[bottom - i][left], 
                    matrix[top][left + i]
                ) = (
                    matrix[top][left + i], 
                    matrix[top + i][right], 
                    matrix[bottom][right - i], 
                    matrix[bottom - i][left]
                ) 
            left += 1
            top += 1
            right -= 1
            bottom -= 1

```

## Key Takeway
이건 로직만 보면 쉬운 문제.

[0, 0] -> [0, n-1]
[0, 1] -> [1, n-1]
이런식의 이동이 이루어진다.

그러나, 문제는 이게 in-place라는 점. 이게 in-place라는건 다시말해 원래 값을 최종 위치 값으로 바로 옮길 수 없다는 뜻이다.
하나의 값을 최종위치로 옮기면 그 최종위치의 있던 값도 바로 최종위치로 가야하고 그 위치에 있던 값도 자기 최종위치로 가야한다.

단, 이거는 90도 턴이고, 따라서 하나의 값을 최종위치로 옮길 때 그에 영향받는 다른 값들도 3번 더 옮기면 문제가 해결된다.
1을 3위치로 옮기면 3을 9위치로 옮기고 9를 7위치로 옮기고 7을 1위치로 옮기면 괜찮다는 뜻.
다행히 파이썬은 이게 정말 쉽다. 

따라서 껍질마다 for문을 돌리면 된다.

이게 근데 헷갈릴 수 있다. 일단 아래와 같이 3단계로 분리해서 생각하는 훈련을 해야 한다.
1)left-top -> right-top, right-top -> right-bottom, right-bottom -> left-bottom, left-bottom -> left-top 이 4개의 운동
2) 위 1번의 운동에서 각 운동에서 변화하는 것이 행인지 열인지 파악해서 행이나 열에서 i를 더하거나 빼주기. (왼->오 운동과 위->아래 운동은 i를 더해주겠지만 나머지 둘은 빼줘야한다)
3) 안쪽 껍질로 이동하기. 이동하기 위해서는 시작값과 끝값 모두 변해야 한다.
우선 시작값만 생각하면, left-top -> right-top이동에서는 시작 행/열이 1칸 늘어야 하고, right-top -> right-bottom이동에서는 시작행만 1칸 늘어나면 된다. 
right-bottom -> left-bottom이동에서는 시작행/열은 늘어날게 없고 left-bottom -> left-top이동에서는 시작열만 1칸 늘어난다. 이렇게 하고서 끝값은 일괄적으로 1씩 줄여야 한다.
이걸 처음해볼땐 실수하기가 쉽다. 특히 여기서 중요한건 끝값을 줄이는 방식이다. 4X4행렬에서는 시작과 끝을 모두 조절한 뒤엔 2X2 행렬을 따져야 하지만,
3X3행렬에서는 시작과 끝을 모두 조절한 뒤에 1X1행렬만 남는다. 즉 끝-시작이 결국 2가 빠져야 한다. 이래서 시작과 끝값 모두 줄어야 한다.
그래서 이걸 간결하게 줄이려면 (원래 길이 - 현재 길이) 같은 방식으로 줄이면 답이 없고... '왼쪽', '오른쪽', '위', '아래'와 같이 포인터 4개를 활용하는게 제일 효과적이다.
여기서 포인터 4개 안고르면 나중에 엄청 헤맨다.. 

파이썬 swapping syntax를 정확히 이해해야 함(우항이 좌항에 대입됨)은 물론이고
괄호 들여쓰기를 잘못하면 4개 값이 한 위치에 다 때려박아지니 그것도 주의해야한다.

각 요소가 한번씩 방문되므로 시간복잡도는 O(M) (M은 요소의 갯수), 공간복잡도는 O(1)이 된다. in-place니까.


# 73. Set Matrix Zeroes

## Solution
```python
class Solution(object):
    def setZeroes(self, matrix):
        is_col = False
        R = len(matrix)
        C = len(matrix[0])

        for i in range(R):
            if matrix[i][0] == 0:
                is_col = True
            for j in range(1, C):
                if matrix[i][j] == 0:
                    matrix[0][j] = 0
                    matrix[i][0] = 0

        for i in range(1, R):
            for j in range(1, C):
                if not matrix[i][0] or not matrix[0][j]:
                    matrix[i][j] = 0

        if matrix[0][0] == 0:
            for j in range(C):
                matrix[0][j] = 0

        if is_col:
            for i in range(R):
                matrix[i][0] = 0
```

## Key Takeway
in-place로 바꿔야 한다.
직관적으로 쉬운 방법은 0으로 바꿀 행과 열을 따로 배열을 만들어서 기억해둔뒤
나중에 바꾸는 것인데, 이러면 메모리를 추가로 사용하게 된다.
공간복잡도 O(1)을 달성하고 싶다면, 이런 추가 배열 없이 작업을 해야한다.

이를 위해 힌트에서도 제시되는 아이디어가 0으로 바꿀 행과 열의 맨 첫번째 값을
일종의 플래그처럼 사용해 0으로 바꿔두고 나중에 다시 첫값이 0인 행과 열을 통째로
0으로 교체해버리는 것.

그런데 이렇게 하려면 구현은 쉬워보여도 행을 한번 죄다 0으로 바꿨는데 하필
첫행이 전부 0으로 바뀌고 나면 0으로 바꿀 열을 골라내지 못하고 전부 0으로 바꿔버리게 된다.

또 설령 어떤 방법을 통해서 그 문제를 해결해서 첫 행 전체를 0으로 바꿔도 성공적으로 필요한 열만 0으로 바꾸게 되었더라도,
여전히 matrix[0][0] 값이 첫 행과 첫 열 둘 중 어느 것을 0으로 바꾸라는 것인지 단정지을 수 없는 문제가 발생한다.
예를 들어, [1 1 1] [1 1 1] [0 1 1] 이런 매트릭스여서 이걸 표시한다고 [0 1 1] [1 1 1] [0 1 1] 이렇게 하면, 나중에 작업하고 나면
[0 0 0] [0 1 1] [0 1 1]이렇게 나와버린다. 원래 원한건 [0 1 1] [0 1 1] [0 1 1]이었음에도.

그래서 이를 해결하기 위해 정답에선 다음 세 가지 작업을 했다.
is_col 불리안 값을 하나 잡아서 정말 첫 열을 0으로 바꿔야하는지 아닌지를 따로 저장하고
첫 열에 0이 있는지 먼저 파악해서 0이 있으면 참으로 바꿔놓고 나중에 첫 열은 따로 바꿔주었다.

처음 0이 있는지 조사할 때 첫열은 제외하고 나머지 열들을 순회해서 그에 따라 행과 열에 0을 표시해주었다.
즉 어떤 n행을 조사할 때, 일단 그 첫값이 0인지를 따져서 is_col을 참으로 업데이트해주고(이미 참이어도 참으로 업데이트),
나머지 값들에 대해서 0이 있으면 그 값의 행과 열의 첫값을 0으로 바꿔주었다.

마지막으로, 첫값들을 참조해서 내용을 0으로 바꿀 때, 첫행과 첫열은 제외하고 나머지 안쪽부분만 바꿔준뒤
따로 for문을 돌려서 첫 행은 matrix[0][0]이 0일 때만, 첫 열은 is_col이 참일때만 전부 0으로 바꿔주었다. 

즉 첫번째 문제는 안쪽먼저 바꾸고 나중에 첫행과 첫열을 고쳐주는 식으로 적용 순서를 구분하여 해결했고,
첫행과 첫열이 matrix[0][0]에 의해 중복대표되는 문제는 불리언 변수 하나를 추가해 해결했다.



