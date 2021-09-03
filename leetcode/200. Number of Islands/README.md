# [200. Number of Islands (Medium)](https://leetcode.com/problems/number-of-islands/)

<p>Given an <code>m x n</code> 2D binary grid <code>grid</code> which represents a map of <code>'1'</code>s (land) and <code>'0'</code>s (water), return <em>the number of islands</em>.</p>

<p>An <strong>island</strong> is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
<strong>Output:</strong> 1
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
<strong>Output:</strong> 3
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 300</code></li>
	<li><code>grid[i][j]</code> is <code>'0'</code> or <code>'1'</code>.</li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Depth-First Search](https://leetcode.com/tag/depth-first-search/), [Breadth-First Search](https://leetcode.com/tag/breadth-first-search/), [Union Find](https://leetcode.com/tag/union-find/), [Matrix](https://leetcode.com/tag/matrix/)

**Similar Questions**:
* [Surrounded Regions (Medium)](https://leetcode.com/problems/surrounded-regions/)
* [Walls and Gates (Medium)](https://leetcode.com/problems/walls-and-gates/)
* [Number of Islands II (Hard)](https://leetcode.com/problems/number-of-islands-ii/)
* [Number of Connected Components in an Undirected Graph (Medium)](https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/)
* [Number of Distinct Islands (Medium)](https://leetcode.com/problems/number-of-distinct-islands/)
* [Max Area of Island (Medium)](https://leetcode.com/problems/max-area-of-island/)
* [Count Sub Islands (Medium)](https://leetcode.com/problems/count-sub-islands/)

## Solution 1. DFS
 
```cpp
// OJ: https://leetcode.com/problems/number-of-islands/
// Author: A M A N
// Time : O(N * M)
// Space: O(N * M)
class Solution {
public:
    void dfs(vector<vector<char>>&grid, vector<vector<bool>>&vis, int x, int y){
        if(x<0 or y<0 or x>=grid.size() or y>=grid[0].size() or vis[x][y] or grid[x][y]=='0')return;
        vis[x][y] = 1;
        dfs(grid, vis, x+1, y);
        dfs(grid, vis, x-1, y);
        dfs(grid, vis, x, y+1);
        dfs(grid, vis, x, y-1);
    }
    int numIslands(vector<vector<char>>& grid) {
        int cnt=0;
        vector<vector<bool>>vis(grid.size(), vector<bool>(grid[0].size(), false));
        for(int i=0; i<grid.size(); i++){
          for(int j=0; j<grid[0].size(); j++){
              if(!vis[i][j] and grid[i][j]=='1'){
                  dfs(grid, vis, i ,j);
                  cnt++;
                }
            }
        }  
        return cnt;
    }
};
```