# [221. Maximal Square (Medium)](https://leetcode.com/problems/maximal-square/)

<p>Given an <code>m x n</code> binary <code>matrix</code> filled with <code>0</code>'s and <code>1</code>'s, <em>find the largest square containing only</em> <code>1</code>'s <em>and return its area</em>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2020/11/26/max1grid.jpg" style="width: 400px; height: 319px;">
<pre><strong>Input:</strong> matrix = [["1","0","1","0","0"],["1","0","1","1","1"],["1","1","1","1","1"],["1","0","0","1","0"]]
<strong>Output:</strong> 4
</pre>

<p><strong>Example 2:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2020/11/26/max2grid.jpg" style="width: 165px; height: 165px;">
<pre><strong>Input:</strong> matrix = [["0","1"],["1","0"]]
<strong>Output:</strong> 1
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> matrix = [["0"]]
<strong>Output:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>m == matrix.length</code></li>
	<li><code>n == matrix[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 300</code></li>
	<li><code>matrix[i][j]</code> is <code>'0'</code> or <code>'1'</code>.</li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/), [Matrix](https://leetcode.com/tag/matrix/)

**Similar Questions**:
* [Maximal Rectangle (Hard)](https://leetcode.com/problems/maximal-rectangle/)
* [Largest Plus Sign (Medium)](https://leetcode.com/problems/largest-plus-sign/)

## Solution 1. DP

<img src="https://assets.leetcode.com/users/arkaung/image_1587997244.png" >

NOTE:
> dp[i][j] means the size of biggest square with matrix[i][j] as bottom-right corner.

> dp[i][j] also means the number of squares with matrix[i][j] as bottom-right corner.

```cpp
// OJ: https://leetcode.com/problems/maximal-square/
// Author: A M A N
// Time : O(N * M)
// Space: O(N * M)
class Solution {
public:
    int maximalSquare(vector<vector<char>>& matrix) {
        int n = matrix.size(), m = matrix[0].size(); bool isThereAny1=false;
        vector<vector<int>> dp(n, vector<int>(m, 0));
        for(int i=0; i<n; i++) dp[i][0] = matrix[i][0] - '0', isThereAny1|=dp[i][0];
        for(int j=0; j<m; j++) dp[0][j] = matrix[0][j] - '0', isThereAny1|=dp[0][j];
        int res = isThereAny1; 
        for(int i=1; i<n; i++)
            for(int j=1; j<m; j++)
                if(matrix[i][j]=='1'){
                    dp[i][j] = 1 + min({dp[i-1][j], dp[i][j-1], dp[i-1][j-1]});
                    res = max(res, dp[i][j]);
                }
        return res*res;
    }
};
```