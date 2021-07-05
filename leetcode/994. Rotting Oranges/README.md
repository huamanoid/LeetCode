# [994. Rotting Oranges (Medium)](https://leetcode.com/problems/rotting-oranges/)

<p>You are given an <code>m x n</code> <code>grid</code> where each cell can have one of three values:</p>

<ul>
	<li><code>0</code> representing an empty cell,</li>
	<li><code>1</code> representing a fresh orange, or</li>
	<li><code>2</code> representing a rotten orange.</li>
</ul>

<p>Every minute, any fresh orange that is <strong>4-directionally adjacent</strong> to a rotten orange becomes rotten.</p>

<p>Return <em>the minimum number of minutes that must elapse until no cell has a fresh orange</em>. If <em>this is impossible, return</em> <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2019/02/16/oranges.png" style="width: 650px; height: 137px;">
<pre><strong>Input:</strong> grid = [[2,1,1],[1,1,0],[0,1,1]]
<strong>Output:</strong> 4
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> grid = [[2,1,1],[0,1,1],[1,0,1]]
<strong>Output:</strong> -1
<strong>Explanation:</strong> The orange in the bottom left corner (row 2, column 0) is never rotten, because rotting only happens 4-directionally.
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> grid = [[0,2]]
<strong>Output:</strong> 0
<strong>Explanation:</strong> Since there are already no fresh oranges at minute 0, the answer is just 0.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 10</code></li>
	<li><code>grid[i][j]</code> is <code>0</code>, <code>1</code>, or <code>2</code>.</li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Breadth-First Search](https://leetcode.com/tag/breadth-first-search/), [Matrix](https://leetcode.com/tag/matrix/)

**Similar Questions**:
* [Walls and Gates (Medium)](https://leetcode.com/problems/walls-and-gates/)

## Solution 1.

```cpp
// OJ: https://leetcode.com/problems/rotting-oranges/
// Author: A M A N
// Time : O(N * M)
// Space: O(N * M)
class Solution {
public:
    int n, m;
    bool isValid(int x, int y){
        if(x<0 or y<0 or x>=n or y>=m)return false;
        return true;
    }
    int orangesRotting(vector<vector<int>>& grid) {
        queue<pair<int, int>>q;
        n = grid.size(), m = grid[0].size();
        for(int i=0; i<n; i++){
            for(int j=0; j<m; j++){
                if(grid[i][j]==2)
                    q.push({i, j});
            }
        }
        while(!q.empty()){
            auto [x, y] = q.front(); q.pop();
            if(isValid(x+1, y) and grid[x+1][y]==1) grid[x+1][y] = 1 + grid[x][y],   q.push({x+1, y});
            if(isValid(x-1, y) and grid[x-1][y]==1) grid[x-1][y] = 1 + grid[x][y],   q.push({x-1, y});
            if(isValid(x, y+1) and grid[x][y+1]==1) grid[x][y+1] = 1 + grid[x][y],   q.push({x, y+1});
            if(isValid(x, y-1) and grid[x][y-1]==1) grid[x][y-1] = 1 + grid[x][y],   q.push({x, y-1});
        }
        int mx = 0;
        for(int i=0; i<n; i++){
            for(int j=0; j<m; j++){
                if(grid[i][j]==1) return -1;
                mx = max(mx, grid[i][j]);
            }
        }
        return max(mx-2, 0);
    }
};
```