# [329. Longest Increasing Path in a Matrix (Hard)](https://leetcode.com/problems/longest-increasing-path-in-a-matrix/)

<p>Given an <code>m x n</code> integers <code>matrix</code>, return <em>the length of the longest increasing path in </em><code>matrix</code>.</p>

<p>From each cell, you can either move in four directions: left, right, up, or down. You <strong>may not</strong> move <strong>diagonally</strong> or move <strong>outside the boundary</strong> (i.e., wrap-around is not allowed).</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/01/05/grid1.jpg" style="width: 242px; height: 242px;">
<pre><strong>Input:</strong> matrix = [[9,9,4],[6,6,8],[2,1,1]]
<strong>Output:</strong> 4
<strong>Explanation:</strong> The longest increasing path is <code>[1, 2, 6, 9]</code>.
</pre>

<p><strong>Example 2:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/01/27/tmp-grid.jpg" style="width: 253px; height: 253px;">
<pre><strong>Input:</strong> matrix = [[3,4,5],[3,2,6],[2,2,1]]
<strong>Output:</strong> 4
<strong>Explanation: </strong>The longest increasing path is <code>[3, 4, 5, 6]</code>. Moving diagonally is not allowed.
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> matrix = [[1]]
<strong>Output:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>m == matrix.length</code></li>
	<li><code>n == matrix[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 200</code></li>
	<li><code>0 &lt;= matrix[i][j] &lt;= 2<sup>31</sup> - 1</code></li>
</ul>


**Companies**:  
[Google](https://leetcode.com/company/google), [Facebook](https://leetcode.com/company/facebook), [Apple](https://leetcode.com/company/apple), [Amazon](https://leetcode.com/company/amazon), [Microsoft](https://leetcode.com/company/microsoft), [Adobe](https://leetcode.com/company/adobe), [Qualtrics](https://leetcode.com/company/qualtrics), [Snapchat](https://leetcode.com/company/snapchat)

**Related Topics**:  
[Dynamic Programming](https://leetcode.com/tag/dynamic-programming/), [Depth-First Search](https://leetcode.com/tag/depth-first-search/), [Breadth-First Search](https://leetcode.com/tag/breadth-first-search/), [Graph](https://leetcode.com/tag/graph/), [Topological Sort](https://leetcode.com/tag/topological-sort/), [Memoization](https://leetcode.com/tag/memoization/)

## Solution 1. DFS + Memoization

```cpp
// OJ: https://leetcode.com/problems/longest-increasing-path-in-a-matrix/
// Author: A M A N
// Time : O(M*N)
// Space: O(M*N)
class Solution {
public:
    int dx[4] = {0, 1, 0, -1};
    int dy[4] = {1, 0, -1, 0};

    int dp[201][201];
    int dfs(vector<vector<int>>&matrix, int x, int y){
        if(dp[x][y]!=-1)
            return dp[x][y];

        int longest = 0;
        for(int i=0; i<4; i++){
            int X = x + dx[i];
            int Y = y + dy[i];
            if(X<0 or X==matrix.size() or Y<0 or Y==matrix[0].size()) continue;
            if(matrix[X][Y]<=matrix[x][y])continue;
            longest = max(longest, 1+dfs(matrix, X, Y));
        }
        return dp[x][y] = longest;
    }

    int longestIncreasingPath(vector<vector<int>>& matrix) {
        memset(dp, -1, sizeof dp);
        int ans = 0;
        for(int i=0; i<matrix.size(); i++){
            for(int j=0; j<matrix[0].size(); j++){
                ans = max(ans, 1+dfs(matrix, i, j));
            }
        }        
        return ans;
    }
};
```