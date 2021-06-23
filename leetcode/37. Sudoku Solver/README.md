# [37. Sudoku Solver (Hard)](https://leetcode.com/problems/sudoku-solver/)

<p>Write a program to solve a Sudoku puzzle by filling the empty cells.</p>

<p>A&nbsp;sudoku solution must satisfy <strong>all of&nbsp;the following rules</strong>:</p>

<ol>
	<li>Each of the digits&nbsp;<code>1-9</code> must occur exactly&nbsp;once in each row.</li>
	<li>Each of the digits&nbsp;<code>1-9</code>&nbsp;must occur&nbsp;exactly once in each column.</li>
	<li>Each of the digits&nbsp;<code>1-9</code> must occur exactly once in each of the 9 <code>3x3</code> sub-boxes of the grid.</li>
</ol>

<p>The <code>'.'</code> character indicates empty cells.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Sudoku-by-L2G-20050714.svg/250px-Sudoku-by-L2G-20050714.svg.png" style="height:250px; width:250px">
<pre><strong>Input:</strong> board = [["5","3",".",".","7",".",".",".","."],["6",".",".","1","9","5",".",".","."],[".","9","8",".",".",".",".","6","."],["8",".",".",".","6",".",".",".","3"],["4",".",".","8",".","3",".",".","1"],["7",".",".",".","2",".",".",".","6"],[".","6",".",".",".",".","2","8","."],[".",".",".","4","1","9",".",".","5"],[".",".",".",".","8",".",".","7","9"]]
<strong>Output:</strong> [["5","3","4","6","7","8","9","1","2"],["6","7","2","1","9","5","3","4","8"],["1","9","8","3","4","2","5","6","7"],["8","5","9","7","6","1","4","2","3"],["4","2","6","8","5","3","7","9","1"],["7","1","3","9","2","4","8","5","6"],["9","6","1","5","3","7","2","8","4"],["2","8","7","4","1","9","6","3","5"],["3","4","5","2","8","6","1","7","9"]]
<strong>Explanation:</strong>&nbsp;The input board is shown above and the only valid solution is shown below:

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/3/31/Sudoku-by-L2G-20050714_solution.svg/250px-Sudoku-by-L2G-20050714_solution.svg.png" style="height:250px; width:250px">
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>board.length == 9</code></li>
	<li><code>board[i].length == 9</code></li>
	<li><code>board[i][j]</code> is a digit or <code>'.'</code>.</li>
	<li>It is <strong>guaranteed</strong> that the input board has only one solution.</li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Backtracking](https://leetcode.com/tag/backtracking/), [Matrix](https://leetcode.com/tag/matrix/)

**Similar Questions**:
* [Valid Sudoku (Medium)](https://leetcode.com/problems/valid-sudoku/)
* [Unique Paths III (Hard)](https://leetcode.com/problems/unique-paths-iii/)

## Solution 1. Backtracking

```cpp
// OJ: https://leetcode.com/problems/sudoku-solver/
// Author: A M A N
// Time: O()
// Space: O()
};
class Solution {
public:
    bool isEmpty(vector<vector<char>>A, int& x, int& y){
    for(int i=0;i<9;i++)
        for(int j=0;j<9;j++)
            if(A[i][j]=='.'){
                x = i;
                y = j;
                return true;
            }
    return false;
}
    bool isValidRow(vector<vector<char>>&A, int x, char num){
        for(int i=0;i<9;i++)
            if(A[x][i]==num)
                return false;
        return true;
    }
    bool isValidCol(vector<vector<char>>&A, int y, char num){
        for(int i=0;i<9;i++)
            if(A[i][y]==num)
                return false;
        return true;
    }
    bool isValidSquare(vector<vector<char>>&A, int x, int y, char num){
        int row = x/3;
        int col = y/3;
        for(int i=3*row;i<3*row+3;i++)
            for(int j=3*col;j<3*col+3;j++)
                if(A[i][j]==num)
                    return false;
        return true;
    }
    bool isValid(vector<vector<char>>&A, int x, int y, char num){
        if(isValidRow(A, x, num) and isValidCol(A, y, num) and isValidSquare(A, x, y, num))
            return true;
        return false;
    }
    bool solve(vector<vector<char>>& A){
        int x,y;
        if(!isEmpty(A, x, y))
            return true;
        for(int i=1;i<=9;i++){
            char num = (i+'0');
```