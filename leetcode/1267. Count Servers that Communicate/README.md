# [1267. Count Servers that Communicate (Medium)](https://leetcode.com/problems/count-servers-that-communicate/)

<p>You are given a map of a server center, represented as a <code>m * n</code> integer matrix&nbsp;<code>grid</code>, where 1 means that on that cell there is a server and 0 means that it is no server. Two servers are said to communicate if they are on the same row or on the same column.<br>
<br>
Return the number of servers&nbsp;that communicate with any other server.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<p><img alt="" src="https://assets.leetcode.com/uploads/2019/11/14/untitled-diagram-6.jpg" style="width: 202px; height: 203px;"></p>

<pre><strong>Input:</strong> grid = [[1,0],[0,1]]
<strong>Output:</strong> 0
<b>Explanation:</b>&nbsp;No servers can communicate with others.</pre>

<p><strong>Example 2:</strong></p>

<p><strong><img alt="" src="https://assets.leetcode.com/uploads/2019/11/13/untitled-diagram-4.jpg" style="width: 203px; height: 203px;"></strong></p>

<pre><strong>Input:</strong> grid = [[1,0],[1,1]]
<strong>Output:</strong> 3
<b>Explanation:</b>&nbsp;All three servers can communicate with at least one other server.
</pre>

<p><strong>Example 3:</strong></p>

<p><img alt="" src="https://assets.leetcode.com/uploads/2019/11/14/untitled-diagram-1-3.jpg" style="width: 443px; height: 443px;"></p>

<pre><strong>Input:</strong> grid = [[1,1,0,0],[0,0,1,0],[0,0,1,0],[0,0,0,1]]
<strong>Output:</strong> 4
<b>Explanation:</b>&nbsp;The two servers in the first row can communicate with each other. The two servers in the third column can communicate with each other. The server at right bottom corner can't communicate with any other server.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m &lt;= 250</code></li>
	<li><code>1 &lt;= n &lt;= 250</code></li>
	<li><code>grid[i][j] == 0 or 1</code></li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Depth-First Search](https://leetcode.com/tag/depth-first-search/), [Breadth-First Search](https://leetcode.com/tag/breadth-first-search/), [Union Find](https://leetcode.com/tag/union-find/), [Matrix](https://leetcode.com/tag/matrix/), [Counting](https://leetcode.com/tag/counting/)

## Solution 1. DFS

```cpp
// OJ: https://leetcode.com/problems/count-servers-that-communicate/
// Author: A M A N
// Time : O(N * M)
// Space: O(N * M)
class Solution {
public:
    void dfs(vector<vector<int>>&grid, vector<vector<bool>>&vis, int x, int y, int& cnt){
        if(x<0 or y<0 or x>=grid.size() or y>=grid[0].size() or !grid[x][y] or vis[x][y])return;
        vis[x][y]=1;
        cnt++;
        for(int i=0; i<grid.size(); i++) //traversing the row
            dfs(grid, vis, i, y, cnt);
        for(int j=0; j<grid[0].size(); j++) // traversing the col
            dfs(grid, vis, x, j, cnt);
    }
    int countServers(vector<vector<int>>& grid) {
        int res=0;
        vector<vector<bool>>vis(grid.size(), vector<bool>(grid[0].size(), false));
        for(int i=0; i<grid.size(); i++){
            for(int j=0; j<grid[0].size(); j++)
                if(grid[i][j] and !vis[i][j]){
                    int cnt =0;
                    dfs(grid, vis, i, j, cnt);
                    if(cnt>1)
                        res+=cnt;
                }
        }
        return res;
    }
};
```