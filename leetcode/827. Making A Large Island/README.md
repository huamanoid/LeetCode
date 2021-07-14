# [827. Making A Large Island (Hard)](https://leetcode.com/problems/making-a-large-island/)

<p>You are given an <code>n x n</code> binary matrix <code>grid</code>. You are allowed to change <strong>at most one</strong> <code>0</code> to be <code>1</code>.</p>

<p>Return <em>the size of the largest <strong>island</strong> in</em> <code>grid</code> <em>after applying this operation</em>.</p>

<p>An <strong>island</strong> is a 4-directionally connected group of <code>1</code>s.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> grid = [[1,0],[0,1]]
<strong>Output:</strong> 3
<strong>Explanation:</strong> Change one 0 to 1 and connect two 1s, then we get an island with area = 3.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> grid = [[1,1],[1,0]]
<strong>Output:</strong> 4
<strong>Explanation: </strong>Change the 0 to 1 and make the island bigger, only one island with area = 4.</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> grid = [[1,1],[1,1]]
<strong>Output:</strong> 4
<strong>Explanation:</strong> Can't change any 0 to 1, only one island with area = 4.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>n == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= n &lt;= 500</code></li>
	<li><code>grid[i][j]</code> is either <code>0</code> or <code>1</code>.</li>
</ul>

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Depth-First Search](https://leetcode.com/tag/depth-first-search/), [Breadth-First Search](https://leetcode.com/tag/breadth-first-search/), [Union Find](https://leetcode.com/tag/union-find/), [Matrix](https://leetcode.com/tag/matrix/)

## Solution 1. DFS

![Idea](https://s3-lc-upload.s3.amazonaws.com/users/votrubac/image_1525310120.png)

```cpp
// OJ: https://leetcode.com/problems/making-a-large-island/
// Author: A M A N
// Time : O(N * M)
// Space: O(N * M)
class Solution {
public:
    unordered_map<int, int> sz;
    void dfs(vector<vector<int>>& grid, int x, int y, int&cnt, int col){
        if(x<0 or y<0 or x>=grid.size() or y>=grid[0].size() or grid[x][y]!=1) return;
        grid[x][y] = col;
        cnt++;
        dfs(grid, x+1, y, cnt, col);
        dfs(grid, x-1, y, cnt, col);
        dfs(grid, x, y+1, cnt, col);
        dfs(grid, x, y-1, cnt, col);
    }
    int largestIsland(vector<vector<int>> grid) {
       int ans =0, col=2, n = grid.size(), m = grid[0].size();
        for(int i=0; i<n; i++){
            for(int j=0; j<m; j++){
                if(grid[i][j]==1){
                    int cnt=0;
                    dfs(grid, i, j, cnt, col); //marking each island with diff color
                    sz[col++] = cnt; // mapping the color to its island size
                }
            }
        }
        sz[0] = 0;
        for(int i=0; i<n; i++)
            for(int j=0; j<m; j++){
                if(grid[i][j]==0){
                    int mx =1; //current 0 cell flipped to 1
                    unordered_set<int>sett; // diff neighbours
                    if(i>0)     sett.insert(grid[i-1][j]);
                    if(i+1<n)   sett.insert(grid[i+1][j]);
                    if(j>0)     sett.insert(grid[i][j-1]);
                    if(j+1<m)   sett.insert(grid[i][j+1]);
                    for(auto a: sett)
                        mx+=sz[a]; 
                    ans = max(ans, mx);
                }
            }
        return ans?ans:n*m; //if there's 0 cell
    }
};
```