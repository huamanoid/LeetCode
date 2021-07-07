# [1020. Number of Enclaves (Medium)](https://leetcode.com/problems/number-of-enclaves/)

<p>You are given an <code>m x n</code> binary matrix <code>grid</code>, where <code>0</code> represents a sea cell and <code>1</code> represents a land cell.</p>

<p>A <strong>move</strong> consists of walking from one land cell to another adjacent (<strong>4-directionally</strong>) land cell or walking off the boundary of the <code>grid</code>.</p>

<p>Return <em>the number of land cells in</em> <code>grid</code> <em>for which we cannot walk off the boundary of the grid in any number of <strong>moves</strong></em>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/02/18/enclaves1.jpg" style="width: 333px; height: 333px;">
<pre><strong>Input:</strong> grid = [[0,0,0,0],[1,0,1,0],[0,1,1,0],[0,0,0,0]]
<strong>Output:</strong> 3
<strong>Explanation:</strong> There are three 1s that are enclosed by 0s, and one 1 that is not enclosed because its on the boundary.
</pre>

<p><strong>Example 2:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/02/18/enclaves2.jpg" style="width: 333px; height: 333px;">
<pre><strong>Input:</strong> grid = [[0,1,1,0],[0,0,1,0],[0,0,1,0],[0,0,0,0]]
<strong>Output:</strong> 0
<strong>Explanation:</strong> All 1s are either on the boundary or can reach the boundary.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 500</code></li>
	<li><code>grid[i][j]</code> is either <code>0</code> or <code>1</code>.</li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Depth-First Search](https://leetcode.com/tag/depth-first-search/), [Breadth-First Search](https://leetcode.com/tag/breadth-first-search/), [Union Find](https://leetcode.com/tag/union-find/), [Matrix](https://leetcode.com/tag/matrix/)

## Solution 1. DFS

```cpp
// OJ: https://leetcode.com/problems/number-of-enclaves/
// Author: A M A N
// Time : O(N * M)
// Space: O(N * M)
class Solution {
public:
    void dfs(vector<vector<int>>&grid, int x, int y, int &cnt){
        if(x<0 or y<0 or x>=grid.size() or y>= grid[0].size() or !grid[x][y])return;
        grid[x][y] = 0;
        cnt++;
        dfs(grid, x, y+1, cnt);
        dfs(grid, x, y-1, cnt);
        dfs(grid, x+1, y, cnt);
        dfs(grid, x-1, y, cnt);
    }
    int numEnclaves(vector<vector<int>>& grid) {
        int n = grid.size(), m = grid[0].size();
        //marking edgeCases as visited
        for(int i=0, c=0; i<n; i++)
            for(int j:{0, m-1})
                dfs(grid, i, j, c);
        for(int j=0, c=0; j<m; j++)
            for(int i:{0, n-1})
                dfs(grid, i, j, c);
        int ans = 0;
        for(int i=0; i<grid.size(); i++){
            for(int j=0; j<grid[0].size(); j++){
                if(grid[i][j]){
                    int cnt=0;
                    dfs(grid, i, j, cnt);
                    ans+=cnt;
                }
            }
        }
        return ans;
    }
};
```