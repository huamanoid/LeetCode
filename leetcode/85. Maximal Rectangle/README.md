# [85. Maximal Rectangle (Hard)](https://leetcode.com/problems/maximal-rectangle/)

<p>Given a <code>rows x cols</code>&nbsp;binary <code>matrix</code> filled with <code>0</code>'s and <code>1</code>'s, find the largest rectangle containing only <code>1</code>'s and return <em>its area</em>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2020/09/14/maximal.jpg" style="width: 402px; height: 322px;">
<pre><strong>Input:</strong> matrix = [["1","0","1","0","0"],["1","0","1","1","1"],["1","1","1","1","1"],["1","0","0","1","0"]]
<strong>Output:</strong> 6
<strong>Explanation:</strong> The maximal rectangle is shown in the above picture.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> matrix = []
<strong>Output:</strong> 0
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> matrix = [["0"]]
<strong>Output:</strong> 0
</pre>

<p><strong>Example 4:</strong></p>

<pre><strong>Input:</strong> matrix = [["1"]]
<strong>Output:</strong> 1
</pre>

<p><strong>Example 5:</strong></p>

<pre><strong>Input:</strong> matrix = [["0","0"]]
<strong>Output:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>rows == matrix.length</code></li>
	<li><code>cols == matrix[i].length</code></li>
	<li><code>0 &lt;= row, cols &lt;= 200</code></li>
	<li><code>matrix[i][j]</code> is <code>'0'</code> or <code>'1'</code>.</li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/), [Stack](https://leetcode.com/tag/stack/), [Matrix](https://leetcode.com/tag/matrix/), [Monotonic Stack](https://leetcode.com/tag/monotonic-stack/)

**Similar Questions**:
* [Largest Rectangle in Histogram (Hard)](https://leetcode.com/problems/largest-rectangle-in-histogram/)
* [Maximal Square (Medium)](https://leetcode.com/problems/maximal-square/)

## Solution 1. 

```cpp
// OJ: https://leetcode.com/problems/maximal-rectangle/
// Author: A M A N
// Time : O(N * M * N)
// Space: O(N * M)
class Solution {
public:
    int maximalRectangle(vector<vector<char>>& matrix) {
        int n = matrix.size();
        if(n==0)return 0;
        int m = matrix[0].size();
        vector<vector<int>> rSum(n, vector<int>(m, 0)); // horizontal continuous no. of 1s ending at i, j
        vector<vector<int>> cSum(n, vector<int>(m, 0)); // vertical continuous no. of 1s ending at i, j 
        for(int i=0; i<n; i++){
            int currSum = 0;
            for(int j=0; j<m; j++){
                if(matrix[i][j]=='0') rSum[i][j] = currSum = 0;
                else                  rSum[i][j] = ++currSum;
            }
        }
        for(int j=0; j<m; j++){
            int currSum = 0;
            for(int i=0; i<n; i++){
                if(matrix[i][j]=='0') cSum[i][j] = currSum = 0;
                else                  cSum[i][j] = ++currSum;
            }
        }
        int ans = 0;
        for(int i=0; i<n; i++){
            for(int j=0; j<m; j++){
                if(matrix[i][j]=='1'){
                    int min_ = INT_MAX;
                    int k = cSum[i][j]; //continous k times 1 in the current column 
                    int area = 0;
                    for(int r=i; r>=i-k+1; r--){ // traverse k times above
                        min_ = min(min_, rSum[r][j]); // calculate running minimum (minimum horizontal area) while traversing above
                        area = max(area, min_*(i-r+1)); // area of the rectangle ending at i,j is [minimum horizontal area * height]
                    }
                    ans = max(area, ans);
                }
            }
        }
        return ans;
    }
};
```