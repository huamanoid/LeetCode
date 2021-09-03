# [304. Range Sum Query 2D - Immutable (Medium)](https://leetcode.com/problems/range-sum-query-2d-immutable/)

<p>Given a 2D matrix <code>matrix</code>, handle multiple queries of the following type:</p>

<ol>
	<li>Calculate the <strong>sum</strong> of the elements of <code>matrix</code> inside the rectangle defined by its <strong>upper left corner</strong> <code>(row1, col1)</code> and <strong>lower right corner</strong> <code>(row2, col2)</code>.</li>
</ol>

<p>Implement the NumMatrix class:</p>

<ul>
	<li><code>NumMatrix(int[][] matrix)</code> Initializes the object with the integer matrix <code>matrix</code>.</li>
	<li><code>int sumRegion(int row1, int col1, int row2, int col2)</code> Returns the <strong>sum</strong> of the elements of <code>matrix</code> inside the rectangle defined by its <strong>upper left corner</strong> <code>(row1, col1)</code> and <strong>lower right corner</strong> <code>(row2, col2)</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/03/14/sum-grid.jpg" style="width: 415px; height: 415px;">
<pre><strong>Input</strong>
["NumMatrix", "sumRegion", "sumRegion", "sumRegion"]
[[[[3, 0, 1, 4, 2], [5, 6, 3, 2, 1], [1, 2, 0, 1, 5], [4, 1, 0, 1, 7], [1, 0, 3, 0, 5]]], [2, 1, 4, 3], [1, 1, 2, 2], [1, 2, 2, 4]]
<strong>Output</strong>
[null, 8, 11, 12]

<strong>Explanation</strong>
NumMatrix numMatrix = new NumMatrix([[3, 0, 1, 4, 2], [5, 6, 3, 2, 1], [1, 2, 0, 1, 5], [4, 1, 0, 1, 7], [1, 0, 3, 0, 5]]);
numMatrix.sumRegion(2, 1, 4, 3); // return 8 (i.e sum of the red rectangle)
numMatrix.sumRegion(1, 1, 2, 2); // return 11 (i.e sum of the green rectangle)
numMatrix.sumRegion(1, 2, 2, 4); // return 12 (i.e sum of the blue rectangle)
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>m == matrix.length</code></li>
	<li><code>n == matrix[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 200</code></li>
	<li><code>-10<sup>5</sup> &lt;= matrix[i][j] &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= row1 &lt;= row2 &lt; m</code></li>
	<li><code>0 &lt;= col1 &lt;= col2 &lt; n</code></li>
	<li>At most <code>10<sup>4</sup></code> calls will be made to <code>sumRegion</code>.</li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Design](https://leetcode.com/tag/design/), [Matrix](https://leetcode.com/tag/matrix/), [Prefix Sum](https://leetcode.com/tag/prefix-sum/)

**Similar Questions**:
* [Range Sum Query - Immutable (Easy)](https://leetcode.com/problems/range-sum-query-immutable/)
* [Range Sum Query 2D - Mutable (Hard)](https://leetcode.com/problems/range-sum-query-2d-mutable/)

## Solution 1. DP

```cpp
// OJ: https://leetcode.com/problems/range-sum-query-2d-immutable/
// Author: A M A N
// Time : O(1)
// Space: O(N * M)
class NumMatrix {
public:
    int dp[201][201];
    NumMatrix(vector<vector<int>>& matrix) {
        memset(dp, 0, sizeof dp);
        int n = matrix.size(), m = matrix[0].size();
        dp[0][0] = matrix[0][0];
        for(int i=1; i<n; i++) dp[i][0] = dp[i-1][0] + matrix[i][0];
        for(int j=1; j<m; j++) dp[0][j] = dp[0][j-1] + matrix[0][j];
        for(int i=1; i<n; i++){
            for(int j=1; j<m; j++){
                dp[i][j] = matrix[i][j] + dp[i-1][j] + dp[i][j-1] - dp[i-1][j-1];
            }
        }
    }
    int sumRegion(int x1, int y1, int x2, int y2) {
        int ans = -1;
        if(x1 and y1)   ans = dp[x2][y2] - dp[x1-1][y2] - dp[x2][y1-1] + dp[x1-1][y1-1];
        else if(x1)     ans = dp[x2][y2] - dp[x1-1][y2];
        else if(y1)     ans = dp[x2][y2] - dp[x2][y1-1];
        else            ans = dp[x2][y2];
        return ans;
    }
};
/**
 * Your NumMatrix object will be instantiated and called as such:
 * NumMatrix* obj = new NumMatrix(matrix);
 * int param_1 = obj->sumRegion(row1,col1,row2,col2);
 */
```