# [463. Island Perimeter (Easy)](https://leetcode.com/problems/island-perimeter/)

<p>You are given <code>row x col</code> <code>grid</code> representing a map where <code>grid[i][j] = 1</code> represents&nbsp;land and <code>grid[i][j] = 0</code> represents water.</p>

<p>Grid cells are connected <strong>horizontally/vertically</strong> (not diagonally). The <code>grid</code> is completely surrounded by water, and there is exactly one island (i.e., one or more connected land cells).</p>

<p>The island doesn't have "lakes", meaning the water inside isn't connected to the water around the island. One cell is a square with side length 1. The grid is rectangular, width and height don't exceed 100. Determine the perimeter of the island.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img src="https://assets.leetcode.com/uploads/2018/10/12/island.png" style="width: 221px; height: 213px;">
<pre><strong>Input:</strong> grid = [[0,1,0,0],[1,1,1,0],[0,1,0,0],[1,1,0,0]]
<strong>Output:</strong> 16
<strong>Explanation:</strong> The perimeter is the 16 yellow stripes in the image above.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> grid = [[1]]
<strong>Output:</strong> 4
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> grid = [[1,0]]
<strong>Output:</strong> 4
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>row == grid.length</code></li>
	<li><code>col == grid[i].length</code></li>
	<li><code>1 &lt;= row, col &lt;= 100</code></li>
	<li><code>grid[i][j]</code> is <code>0</code> or <code>1</code>.</li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Depth-First Search](https://leetcode.com/tag/depth-first-search/), [Breadth-First Search](https://leetcode.com/tag/breadth-first-search/), [Matrix](https://leetcode.com/tag/matrix/)

**Similar Questions**:
* [Max Area of Island (Medium)](https://leetcode.com/problems/max-area-of-island/)
* [Flood Fill (Easy)](https://leetcode.com/problems/flood-fill/)
* [Coloring A Border (Medium)](https://leetcode.com/problems/coloring-a-border/)

## Solution 1. DFS

```cpp
// OJ: https://leetcode.com/problems/island-perimeter/
// Author: A M A N
// Time : O(N * M)
// Space: O(N * M)
class Solution {
public:
    int peri=0;
    int dx[4] = {0, 1, 0, -1};
    int dy[4] = {1, 0, -1, 0};
    bool isValid(vector<vector<int>>&grid, int x, int y){
        if(x<0 or y<0 or x>=grid.size() or y>= grid[0].size())
            return false;
        return true;
    }
    void dfs(vector<vector<int>>& grid, vector<vector<int>>&vis, int x, int y){
        if(!isValid(grid, x, y))return;
        if(!grid[x][y]) return;
        if(vis[x][y]) return;
        vis[x][y] = 1;
        int edge = 0;
        for(int i=0; i<4; i++){
            int X = x + dx[i];
            int Y = y + dy[i];
            dfs(grid, vis, X, Y);
            if(!isValid(grid, X, Y) or !grid[X][Y])edge++;
        }
        peri += edge;
    }
    int islandPerimeter(vector<vector<int>>& grid) {
        vector<vector<int>> vis(grid.size(), vector<int>(grid[0].size(), 0));
        for(int i=0; i<grid.size(); i++){
            for(int j=0; j<grid[i].size(); j++){
                if(!vis[i][j])
                    dfs(grid, vis, i, j);
            }
        }
        return peri;
    }
};
```