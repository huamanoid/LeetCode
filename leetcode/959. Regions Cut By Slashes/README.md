# [959. Regions Cut By Slashes (Medium)](https://leetcode.com/problems/regions-cut-by-slashes/)

<p>An <code>n x n</code> grid is composed of <code>1 x 1</code> squares where each <code>1 x 1</code> square consists of a <code>'/'</code>, <code>'\'</code>, or blank space <code>' '</code>. These characters divide the square into contiguous regions.</p>

<p>Given the grid <code>grid</code> represented as a string array, return <em>the number of regions</em>.</p>

<p>Note that backslash characters are escaped, so a <code>'\'</code> is represented as <code>'\\'</code>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2018/12/15/1.png" style="width: 200px; height: 200px;">
<pre><strong>Input:</strong> grid = [" /","/ "]
<strong>Output:</strong> 2
</pre>

<p><strong>Example 2:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2018/12/15/2.png" style="width: 200px; height: 198px;">
<pre><strong>Input:</strong> grid = [" /","  "]
<strong>Output:</strong> 1
</pre>

<p><strong>Example 3:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2018/12/15/3.png" style="width: 200px; height: 198px;">
<pre><strong>Input:</strong> grid = ["\\/","/\\"]
<strong>Output:</strong> 4
<strong>Explanation: </strong>(Recall that because \ characters are escaped, "\\/" refers to \/, and "/\\" refers to /\.)
</pre>

<p><strong>Example 4:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2018/12/15/4.png" style="width: 200px; height: 200px;">
<pre><strong>Input:</strong> grid = ["/\\","\\/"]
<strong>Output:</strong> 5
<strong>Explanation: </strong>(Recall that because \ characters are escaped, "\\/" refers to \/, and "/\\" refers to /\.)
</pre>

<p><strong>Example 5:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2018/12/15/5.png" style="width: 200px; height: 200px;">
<pre><strong>Input:</strong> grid = ["//","/ "]
<strong>Output:</strong> 3
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>n == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= n &lt;= 30</code></li>
	<li><code>grid[i][j]</code> is either <code>'/'</code>, <code>'\'</code>, or <code>' '</code>.</li>
</ul>


**Related Topics**:  
[Depth-First Search](https://leetcode.com/tag/depth-first-search/), [Breadth-First Search](https://leetcode.com/tag/breadth-first-search/), [Union Find](https://leetcode.com/tag/union-find/), [Graph](https://leetcode.com/tag/graph/)

## Solution 1. DFS
```
converting '/' to 
0 0 1 
0 1 0 
1 0 0

and '\' to
1 0 0
0 1 0
0 0 1
```

```cpp
// OJ: https://leetcode.com/problems/regions-cut-by-slashes/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    void dfs(vector<vector<int>>& g, vector<vector<bool>>&vis, int x, int y){
        if(x<0 or y<0 or x>=g.size() or y>=g.size() or vis[x][y] or g[x][y]==1)
            return;
        vis[x][y] = 1;
        dfs(g, vis, x+1, y);
        dfs(g, vis, x, y+1);
        dfs(g, vis, x-1, y);
        dfs(g, vis, x, y-1);
    }
    int regionsBySlashes(vector<string>& grid) {
        vector<vector<int>> g(3*grid.size(), vector<int>(3*grid.size(), 0));
        for(int i=0; i<grid.size(); i++){
            for(int j=0; j<grid.size(); j++){
                if(grid[i][j]=='/'){
                    g[3*i][3*j+2] = 1;
                    g[3*i+1][3*j+1] = 1;
                    g[3*i+2][3*j] = 1;
                }else if(grid[i][j]=='\\'){
                    g[3*i][3*j] = 1;
                    g[3*i+1][3*j+1] = 1;
                    g[3*i+2][3*j+2] = 1;
                }
            }
        }
        int cnt=0;
        vector<vector<bool>> vis(g.size(), vector<bool>(g.size(), 0));
        for(int i=0; i<g.size(); i++){
            for(int j=0; j<g.size(); j++){
                cout<<g[i][j]<<" ";
                if(!vis[i][j] and g[i][j]==0){
                    dfs(g, vis, i, j);
                    cnt++;
                }
            }
            cout<<endl;
        }
        return cnt;
    }
};
```