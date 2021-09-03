# [73. Set Matrix Zeroes (Medium)](https://leetcode.com/problems/set-matrix-zeroes/)

<p>Given an <code>m x n</code> integer matrix <code>matrix</code>, if an element is <code>0</code>, set its entire row and column to <code>0</code>'s, and return <em>the matrix</em>.</p>

<p>You must do it <a href="https://en.wikipedia.org/wiki/In-place_algorithm" target="_blank">in place</a>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2020/08/17/mat1.jpg" style="width: 450px; height: 169px;">
<pre><strong>Input:</strong> matrix = [[1,1,1],[1,0,1],[1,1,1]]
<strong>Output:</strong> [[1,0,1],[0,0,0],[1,0,1]]
</pre>

<p><strong>Example 2:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2020/08/17/mat2.jpg" style="width: 450px; height: 137px;">
<pre><strong>Input:</strong> matrix = [[0,1,2,0],[3,4,5,2],[1,3,1,5]]
<strong>Output:</strong> [[0,0,0,0],[0,4,5,0],[0,3,1,0]]
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>m == matrix.length</code></li>
	<li><code>n == matrix[0].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 200</code></li>
	<li><code>-2<sup>31</sup> &lt;= matrix[i][j] &lt;= 2<sup>31</sup> - 1</code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Follow up:</strong></p>

<ul>
	<li>A straightforward solution using <code>O(mn)</code> space is probably a bad idea.</li>
	<li>A simple improvement uses <code>O(m + n)</code> space, but still not the best solution.</li>
	<li>Could you devise a constant space solution?</li>
</ul>


**Companies**:  
[Microsoft](https://leetcode.com/company/microsoft), [Facebook](https://leetcode.com/company/facebook), [Amazon](https://leetcode.com/company/amazon), [Qualtrics](https://leetcode.com/company/qualtrics)

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Hash Table](https://leetcode.com/tag/hash-table/), [Matrix](https://leetcode.com/tag/matrix/)

**Similar Questions**:
* [Game of Life (Medium)](https://leetcode.com/problems/game-of-life/)

## Solution 1.

```cpp
// OJ: https://leetcode.com/problems/set-matrix-zeroes/
// Author: A M A N
// Time : O(N*M)
// Space: O(N)
class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {
        unordered_set<int> row, col;
        for(int i=0; i<matrix.size(); i++){
            for(int j=0; j<matrix[0].size(); j++){
                if(!matrix[i][j])
                    row.insert(i), col.insert(j);
            }
        }
        for(int i: row){
            for(int j=0; j<matrix[0].size(); j++)
                matrix[i][j] = 0;
        }
        for(int j: col){
            for(int i=0; i<matrix.size(); i++)
                matrix[i][j] = 0;
        }
    }
};
```


## Solution 2. 

```cpp
// OJ: https://leetcode.com/problems/set-matrix-zeroes/
// Author: A M A N
// Time : O(N*M)
// Space: O(1)
class Solution {
public:
    const int M = 1e9+13; // A number that doesnt exist in the matrix
    void setZeroes(vector<vector<int>>& matrix) {
        for(int i=0; i<matrix.size(); i++){
            for(int j=0; j<matrix[0].size(); j++){
                if(!matrix[i][j])
                    matrix[i][j] = M;
            }
        }
       for(int i=0; i<matrix.size(); i++){
           for(int j=0; j<matrix[0].size(); j++){
               if(matrix[i][j]==M){
                //set ith row zero
                   for(int k=0; k<matrix[0].size(); k++)
                       if(matrix[i][k]!=M)
                        matrix[i][k] = 0;
                //set jth col zero
                   for(int k=0; k<matrix.size(); k++)
                       if(matrix[k][j]!=M)
                       matrix[k][j] = 0;
               }
           }
       }
        for(int i=0; i<matrix.size(); i++)
            for(int j=0; j<matrix[0].size(); j++)
                if(matrix[i][j]==M)
                    matrix[i][j] = 0;
    }
};
```

## Solution 3.

To solve it in a constant space, we can use the first cell of each row and col as a flag.
This flag would determine whether a row or column has been set to zero.
As if there's a zero in a row or col, the first element is going to be vanished anyway so we can modify it without of any loss of data. 
Because the state of row0 and the state of column0 would occupy the same cell, I let it be the state of row0, and use another variable "col0" for column0.
```cpp
// OJ: https://leetcode.com/problems/set-matrix-zeroes/
// Author: A M A N
// Time : O(N*M)
// Space: O(1)
class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {
        int col0 = 1, n = matrix.size(), m = matrix[0].size();
        for (int i = 0; i < n; i++) {
            if (matrix[i][0] == 0) col0 = 0;
            for (int j = 1; j < m; j++)
                if (matrix[i][j] == 0)
                    matrix[i][0] = matrix[0][j] = 0;
        }
        //traversing in a bottom up manner to avoid modifying the flag values without even using it 
        for (int i = n - 1; i >= 0; i--) {
            for (int j = m - 1; j >= 1; j--)
                if (matrix[i][0] == 0 or matrix[0][j] == 0)
                    matrix[i][j] = 0;
            if (col0 == 0) matrix[i][0] = 0;
        }
    }
};
```