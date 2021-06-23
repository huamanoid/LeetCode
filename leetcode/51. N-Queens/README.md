# [51. N-Queens (Hard)](https://leetcode.com/problems/n-queens/)

<p>The <strong>n-queens</strong> puzzle is the problem of placing <code>n</code> queens on an <code>n x n</code> chessboard such that no two queens attack each other.</p>

<p>Given an integer <code>n</code>, return <em>all distinct solutions to the <strong>n-queens puzzle</strong></em>. You may return the answer in <strong>any order</strong>.</p>

<p>Each solution contains a distinct board configuration of the n-queens' placement, where <code>'Q'</code> and <code>'.'</code> both indicate a queen and an empty space, respectively.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2020/11/13/queens.jpg" style="width: 600px; height: 268px;">
<pre><strong>Input:</strong> n = 4
<strong>Output:</strong> [[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]
<strong>Explanation:</strong> There exist two distinct solutions to the 4-queens puzzle as shown above
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> n = 1
<strong>Output:</strong> [["Q"]]
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 9</code></li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Backtracking](https://leetcode.com/tag/backtracking/)

**Similar Questions**:
* [N-Queens II (Hard)](https://leetcode.com/problems/n-queens-ii/)
* [Grid Illumination (Hard)](https://leetcode.com/problems/grid-illumination/)

## Solution 1. Backtracking

```cpp
// OJ: https://leetcode.com/problems/n-queens/
// Author: A M A N
//Time:  O(N!)
//Space: O(N^2)
};
class Solution {
public:
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
```