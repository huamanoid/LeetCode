# [52. N-Queens II (Hard)](https://leetcode.com/problems/n-queens-ii/)

<p>The <strong>n-queens</strong> puzzle is the problem of placing <code>n</code> queens on an <code>n x n</code> chessboard such that no two queens attack each other.</p>

<p>Given an integer <code>n</code>, return <em>the number of distinct solutions to the&nbsp;<strong>n-queens puzzle</strong></em>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2020/11/13/queens.jpg" style="width: 600px; height: 268px;">
<pre><strong>Input:</strong> n = 4
<strong>Output:</strong> 2
<strong>Explanation:</strong> There are two distinct solutions to the 4-queens puzzle as shown.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> n = 1
<strong>Output:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 9</code></li>
</ul>


**Related Topics**:  
[Backtracking](https://leetcode.com/tag/backtracking/)

**Similar Questions**:
* [N-Queens (Hard)](https://leetcode.com/problems/n-queens/)

## Solution 1. Backtracking

```cpp
// OJ: https://leetcode.com/problems/n-queens-ii/
// Author: A M A N
//Time:  O(N!)
//Space: O(N^2)
class Solution {
public:
    int solutionCount;
    vector<vector<string>> ans;
    vector<string> makeGrid(int n){
        vector<string> res;
        string s(n, '.');
        while(n--)
            res.push_back(s);
        return res;
    }
    bool isValid(vector<string> grid, int r, int c){
//         valid col?
        for(string s: grid){
            if(s[c]=='Q')
                return false;
        }
//         valid left diagonal
        for(int i=r, j=c; i>=0 and j>=0; i--, j--)
            if(grid[i][j]=='Q')
                return false;
//         valid right diagonal
        for(int i=r, j=c; i>=0 and j<grid.size(); i--,j++)
            if(grid[i][j]=='Q')
                return false;
        return true;
    }
    void solve(vector<string> grid, int row, int n){
        if(row==n){
            ans.push_back(grid);
            solutionCount++;
            return;
        }
        for(int j = 0; j<grid.size(); j++){
            if(!isValid(grid, row, j))continue;
            grid[row][j] = 'Q';
            solve(grid, row+1, n);
            grid[row][j] = '.';
        }
        return;
    }
    int totalNQueens(int n) {
        vector<string> grid = makeGrid(n);
        solve(grid, 0, n);
        return solutionCount;
    }
};
```