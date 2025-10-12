# 36. Valida Sudoku

Determine if a 9 x 9 Sudoku board is valid. Only the filled cells need to be validated according to the following rules:

    Each row must contain the digits 1-9 without repetition.
    Each column must contain the digits 1-9 without repetition.
    Each of the nine 3 x 3 sub-boxes of the grid must contain the digits 1-9 without repetition.

Note:

    A Sudoku board (partially filled) could be valid but is not necessarily solvable.
    Only the filled cells need to be validated according to the mentioned rules.

Example 1:

Input: board = 
[["5","3",".",".","7",".",".",".","."]
,["6",".",".","1","9","5",".",".","."]
,[".","9","8",".",".",".",".","6","."]
,["8",".",".",".","6",".",".",".","3"]
,["4",".",".","8",".","3",".",".","1"]
,["7",".",".",".","2",".",".",".","6"]
,[".","6",".",".",".",".","2","8","."]
,[".",".",".","4","1","9",".",".","5"]
,[".",".",".",".","8",".",".","7","9"]]

Output: true

## Solution

```cpp
class Solution {
public:
    bool isValidSudoku(vector<vector<char>>& board) {
        for(auto i=0;i<9;i+=3) {
            for(auto k=0;k<9;k+=3) {
                if(check3x3(board, i, k) == false) 
                    return false;
            }
        }

        for(auto i=0;i<9;i++) {
            if(checkCol(board, i) == false)
                return false;

            if(checkRow(board, i) == false)
                return false;
        }

        return true;
    }

    bool check3x3(vector<vector<char>>& board, int _x, int _y) {
        auto m = map<char, bool>();

        for(auto x=0;x<3;x++) {
            for(auto y=0;y<3;y++) {
                auto c = board[x+_x][y+_y];
                
                if(c == '.') continue;
                else if(m.find(c) != m.end()) return false;

                m[c] = true;
            }
        }

        return true;
    }

    bool checkRow(vector<vector<char>>& board, int i) {
        auto m = map<char, bool>();

        for(auto x=0;x<9;x++) {
                auto c = board[i][x];
                
                if(c == '.') continue;
                else if(m.find(c) != m.end()) return false;

                cout<<c<<endl;

                m[c] = true;
        }

        return true;
    }

    bool checkCol(vector<vector<char>>& board, int i) {
        auto m = map<char, bool>();

        for(auto y=0;y<9;y++) {
                auto c = board[y][i];
                
                if(c == '.') continue;
                else if(m.find(c) != m.end()) return false;

                cout<<c<<endl;

                m[c] = true;
        }

        return true;
    }
};
```
# 37.Sudoku Solver

Write a program to solve a Sudoku puzzle by filling the empty cells.

A sudoku solution must satisfy all of the following rules:

    Each of the digits 1-9 must occur exactly once in each row.
    Each of the digits 1-9 must occur exactly once in each column.
    Each of the digits 1-9 must occur exactly once in each of the 9 3x3 sub-boxes of the grid.

The '.' character indicates empty cells.

Example 1:

Input: board = [["5","3",".",".","7",".",".",".","."],["6",".",".","1","9","5",".",".","."],[".","9","8",".",".",".",".","6","."],["8",".",".",".","6",".",".",".","3"],["4",".",".","8",".","3",".",".","1"],["7",".",".",".","2",".",".",".","6"],[".","6",".",".",".",".","2","8","."],[".",".",".","4","1","9",".",".","5"],[".",".",".",".","8",".",".","7","9"]]

Output: [["5","3","4","6","7","8","9","1","2"],["6","7","2","1","9","5","3","4","8"],["1","9","8","3","4","2","5","6","7"],["8","5","9","7","6","1","4","2","3"],["4","2","6","8","5","3","7","9","1"],["7","1","3","9","2","4","8","5","6"],["9","6","1","5","3","7","2","8","4"],["2","8","7","4","1","9","6","3","5"],["3","4","5","2","8","6","1","7","9"]]

Explanation: The input board is shown above and the only valid solution is shown below:

## Solution

```cpp
using Board = vector<vector<char>>;

void solveSudoku(Board& board) {
    trySudoku(board, 0, 0);
}

bool trySudoku(Board& board, int x, int y) {
    if(x == 9) {
        return true;
    }
    
    auto nx = y == 8 ? x + 1 : x;
    auto ny = y == 8 ? 0 : y + 1;

    if(board[x][y] != '.') return trySudoku(board, nx, ny);

    for(auto i=1;i<10;i++) {
        if(checkSudoku(board, x, y, i) == false) continue;
        board[x][y] = i + '0';
        if(trySudoku(board, nx, ny)) return true;
        board[x][y] = '.';
    }

    return false;
}

bool checkSudoku(Board& board, int x, int y, int num) {
    auto r = (x / 3) * 3;
    auto c = (y / 3) * 3;

    char tar = num +'0';

    for(auto i=0;i<9;i++) {
        if(board[x][i] == tar) return false;
        if(board[i][y] == tar) return false;

        auto gx = (i/3) + r;
        auto gy = i % 3 + c;
        if(board[gx][gy] == tar) return false;
    }

    return true;
}
```

# 38. Count and Say

The count-and-say sequence is a sequence of digit strings defined by the recursive formula:

    countAndSay(1) = "1"
    countAndSay(n) is the run-length encoding of countAndSay(n - 1).

Run-length encoding (RLE) is a string compression method that works by replacing consecutive identical characters (repeated 2 or more times) with the concatenation of the character and the number marking the count of the characters (length of the run). For example, to compress the string "3322251" we replace "33" with "23", replace "222" with "32", replace "5" with "15" and replace "1" with "11". Thus the compressed string becomes "23321511".

Given a positive integer n, return the nth element of the count-and-say sequence.

Example 1:

Input: n = 4

Output: "1211"

Explanation:

countAndSay(1) = "1"
countAndSay(2) = RLE of "1" = "11"
countAndSay(3) = RLE of "11" = "21"
countAndSay(4) = RLE of "21" = "1211"

Example 2:

Input: n = 1

Output: "1"

Explanation:

This is the base case.

## Solution

```cpp
string countAndSay(int n) {
    if(n == 1) return "1";

    auto str = countAndSay(n - 1);
    auto pre = '.';
    auto cnt = 0;

    auto ret = std::string();

    for(auto c : str) {
        if(pre == c) {
            cout<<pre<<endl;
            cnt++;
        }
        else {
            if(cnt != 0) ret += to_string(cnt) + pre;
            pre = c;
            cnt = 1;
        }

    }

    ret += to_string(cnt) + pre;

    return ret;
}
```