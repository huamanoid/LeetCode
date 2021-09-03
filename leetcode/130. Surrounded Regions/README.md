# [130. Surrounded Regions (Medium)](https://leetcode.com/problems/surrounded-regions/)

<p>Given an <code>m x n</code> matrix <code>board</code> containing <code>'X'</code> and <code>'O'</code>, <em>capture all regions surrounded by</em> <code>'X'</code>.</p>

<p>A region is <strong>captured</strong> by flipping all <code>'O'</code>s into <code>'X'</code>s in that surrounded region.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/02/19/xogrid.jpg" style="width: 550px; height: 237px;">
<pre><strong>Input:</strong> board = [["X","X","X","X"],["X","O","O","X"],["X","X","O","X"],["X","O","X","X"]]
<strong>Output:</strong> [["X","X","X","X"],["X","X","X","X"],["X","X","X","X"],["X","O","X","X"]]
<strong>Explanation:</strong> Surrounded regions should not be on the border, which means that any 'O' on the border of the board are not flipped to 'X'. Any 'O' that is not on the border and it is not connected to an 'O' on the border will be flipped to 'X'. Two cells are connected if they are adjacent cells connected horizontally or vertically.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> board = [["X"]]
<strong>Output:</strong> [["X"]]
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>m == board.length</code></li>
	<li><code>n == board[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 200</code></li>
	<li><code>board[i][j]</code> is <code>'X'</code> or <code>'O'</code>.</li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Depth-First Search](https://leetcode.com/tag/depth-first-search/), [Breadth-First Search](https://leetcode.com/tag/breadth-first-search/), [Union Find](https://leetcode.com/tag/union-find/), [Matrix](https://leetcode.com/tag/matrix/)

**Similar Questions**:
* [Number of Islands (Medium)](https://leetcode.com/problems/number-of-islands/)
* [Walls and Gates (Medium)](https://leetcode.com/problems/walls-and-gates/)

## Solution 1. DFS

```cpp
// OJ: https://leetcode.com/problems/surrounded-regions/
// Author: A M A N
// Time : O(N * M)
// Space: O(N * M)
class Solution {
public:
    void dfs(vector<vector<char>>& board, int x, int y){
        if(x<0 or y<0 or x>=board.size() or y>=board[0].size() or board[x][y]!='O')return;
        board[x][y] = '+';
        dfs(board, x+1, y);
        dfs(board, x-1, y);
        dfs(board, x, y+1);
        dfs(board, x, y-1);
    }
    void solve(vector<vector<char>>& board) {
        int n = board.size();
        int m = board[0].size();
        for(int i=0; i<n; i++)  dfs(board, i, 0), dfs(board, i, m-1); //components of 'O' which are connected with the edges 
        for(int j=0; j<m; j++)  dfs(board, 0, j), dfs(board, n-1, j); //will be saved at the end
        for(int i=0; i<n; i++)
            for(int j=0; j<m; j++)
                board[i][j] = (board[i][j]=='+'? 'O':'X'); 
    }
};
```