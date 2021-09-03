# [1277. Count Square Submatrices with All Ones (Medium)](https://leetcode.com/problems/count-square-submatrices-with-all-ones/)

<p>Given a <code>m * n</code> matrix of ones and zeros, return how many <strong>square</strong> submatrices have all ones.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> matrix =
[
&nbsp; [0,1,1,1],
&nbsp; [1,1,1,1],
&nbsp; [0,1,1,1]
]
<strong>Output:</strong> 15
<strong>Explanation:</strong> 
There are <strong>10</strong> squares of side 1.
There are <strong>4</strong> squares of side 2.
There is  <strong>1</strong> square of side 3.
Total number of squares = 10 + 4 + 1 = <strong>15</strong>.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> matrix = 
[
  [1,0,1],
  [1,1,0],
  [1,1,0]
]
<strong>Output:</strong> 7
<strong>Explanation:</strong> 
There are <b>6</b> squares of side 1.  
There is <strong>1</strong> square of side 2. 
Total number of squares = 6 + 1 = <b>7</b>.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= arr.length&nbsp;&lt;= 300</code></li>
	<li><code>1 &lt;= arr[0].length&nbsp;&lt;= 300</code></li>
	<li><code>0 &lt;= arr[i][j] &lt;= 1</code></li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/), [Matrix](https://leetcode.com/tag/matrix/)

## Solution 1. DP

> dp[i][j] means the size of biggest square with matrix[i][j] as bottom-right corner.

> dp[i][j] also means the number of squares with matrix[i][j] as bottom-right corner.

```cpp
// OJ: https://leetcode.com/problems/count-square-submatrices-with-all-ones/
// Author: A M A N
// Time : O(N*M)
// Space: O(N*M) can be reduced to O(1) by editing the given matrix
class Solution {
public:
    int countSquares(vector<vector<int>>& matrix) {
        int n = matrix.size(), m = matrix[0].size();
        vector<vector<int>> dp(n, vector<int>(m, 0));
        for(int i=0; i<n; i++)dp[i][0] = matrix[i][0];
        for(int i=0; i<m; i++)dp[0][i] = matrix[0][i];
        for(int i=1; i<n; i++){
            for(int j=1; j<m; j++){
                if(matrix[i][j]){
                    dp[i][j] = 1 + min({dp[i-1][j], dp[i-1][j-1], dp[i][j-1]});
                }
            }
        }
        int cnt = 0;
        for(int i=0; i<n; i++)
            cnt+= accumulate(dp[i].begin(), dp[i].end(), 0);
        return cnt;
    }
};
```