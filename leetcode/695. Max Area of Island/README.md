# [695. Max Area of Island (Medium)](https://leetcode.com/problems/max-area-of-island/)

<p>You are given an <code>m x n</code> binary matrix <code>grid</code>. An island is a group of <code>1</code>'s (representing land) connected <strong>4-directionally</strong> (horizontal or vertical.) You may assume all four edges of the grid are surrounded by water.</p>

<p>The <strong>area</strong> of an island is the number of cells with a value <code>1</code> in the island.</p>

<p>Return <em>the maximum <strong>area</strong> of an island in </em><code>grid</code>. If there is no island, return <code>0</code>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/05/01/maxarea1-grid.jpg" style="width: 500px; height: 310px;">
<pre><strong>Input:</strong> grid = [[0,0,1,0,0,0,0,1,0,0,0,0,0],[0,0,0,0,0,0,0,1,1,1,0,0,0],[0,1,1,0,1,0,0,0,0,0,0,0,0],[0,1,0,0,1,1,0,0,1,0,1,0,0],[0,1,0,0,1,1,0,0,1,1,1,0,0],[0,0,0,0,0,0,0,0,0,0,1,0,0],[0,0,0,0,0,0,0,1,1,1,0,0,0],[0,0,0,0,0,0,0,1,1,0,0,0,0]]
<strong>Output:</strong> 6
<strong>Explanation:</strong> The answer is not 11, because the island must be connected 4-directionally.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> grid = [[0,0,0,0,0,0,0,0]]
<strong>Output:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 50</code></li>
	<li><code>grid[i][j]</code> is either <code>0</code> or <code>1</code>.</li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Depth-First Search](https://leetcode.com/tag/depth-first-search/), [Breadth-First Search](https://leetcode.com/tag/breadth-first-search/), [Union Find](https://leetcode.com/tag/union-find/), [Matrix](https://leetcode.com/tag/matrix/)

**Similar Questions**:
* [Number of Islands (Medium)](https://leetcode.com/problems/number-of-islands/)
* [Island Perimeter (Easy)](https://leetcode.com/problems/island-perimeter/)
* [Largest Submatrix With Rearrangements (Medium)](https://leetcode.com/problems/largest-submatrix-with-rearrangements/)

## Solution 1. DFS

```cpp
// OJ: https://leetcode.com/problems/max-area-of-island/
// Author: A M A N
// Time : O(N * M)
// Space: O(N * M)
class Solution {
public:
    void dfs(vector<vector<int>>&grid, vector<vector<bool>>&vis, int x, int y, int& cnt){
        if(x<0 or y<0 or x>=grid.size() or y>=grid[0].size() or vis[x][y] or !grid[x][y])return;
        vis[x][y] = 1;
        cnt++;
        dfs(grid, vis, x+1, y, cnt);
        dfs(grid, vis, x-1, y, cnt);
        dfs(grid, vis, x, y+1, cnt);
        dfs(grid, vis, x, y-1, cnt);
    }
    int maxAreaOfIsland(vector<vector<int>>& grid) {
        int ans = 0;
        vector<vector<bool>>vis(grid.size(), vector<bool>(grid[0].size(), false));
        for(int i=0; i<grid.size(); i++){
            for(int j=0; j<grid[i].size(); j++){
                if(!vis[i][j] and grid[i][j]){
                    int cnt=0;
                    dfs(grid, vis, i, j, cnt);
                    ans = max(cnt, ans);
                }
            }
        }    
        return ans;
    }
};
```