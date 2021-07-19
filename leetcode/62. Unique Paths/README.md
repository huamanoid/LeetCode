# [62. Unique Paths (Medium)](https://leetcode.com/problems/unique-paths/)

<p>A robot is located at the top-left corner of a <code>m x n</code> grid (marked 'Start' in the diagram below).</p>

<p>The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).</p>

<p>How many possible unique paths are there?</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img src="https://assets.leetcode.com/uploads/2018/10/22/robot_maze.png" style="width: 400px; height: 183px;">
<pre><strong>Input:</strong> m = 3, n = 7
<strong>Output:</strong> 28
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> m = 3, n = 2
<strong>Output:</strong> 3
<strong>Explanation:</strong>
From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
1. Right -&gt; Down -&gt; Down
2. Down -&gt; Down -&gt; Right
3. Down -&gt; Right -&gt; Down
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> m = 7, n = 3
<strong>Output:</strong> 28
</pre>

<p><strong>Example 4:</strong></p>

<pre><strong>Input:</strong> m = 3, n = 3
<strong>Output:</strong> 6
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= m, n &lt;= 100</code></li>
	<li>It's guaranteed that the answer will be less than or equal to <code>2 * 10<sup>9</sup></code>.</li>
</ul>


**Related Topics**:  
[Math](https://leetcode.com/tag/math/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/), [Combinatorics](https://leetcode.com/tag/combinatorics/)

**Similar Questions**:
* [Unique Paths II (Medium)](https://leetcode.com/problems/unique-paths-ii/)
* [Minimum Path Sum (Medium)](https://leetcode.com/problems/minimum-path-sum/)
* [Dungeon Game (Hard)](https://leetcode.com/problems/dungeon-game/)

## Solution 1. DP

```cpp
// OJ: https://leetcode.com/problems/unique-paths/
// Author: A M A N
// Time : O(N * M)
// Space: O(N * M)
class Solution {
public:
    int uniquePaths(int n, int m) {
        vector<vector<int>>dp(n, vector<int>(m, 0));
        for(int i=0; i<n; i++) dp[i][0] = 1;
        for(int j=0; j<m; j++) dp[0][j] = 1;
        for(int i=1; i<n; i++){
            for(int j=1; j<m; j++){
                dp[i][j] = dp[i-1][j] + dp[i][j-1];
            }
        }
        return dp[n-1][m-1];
    }
};
```