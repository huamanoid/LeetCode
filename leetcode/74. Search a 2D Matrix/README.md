# [74. Search a 2D Matrix (Medium)](https://leetcode.com/problems/search-a-2d-matrix/)

<p>Write an efficient algorithm that searches for a value in an <code>m x n</code> matrix. This matrix has the following properties:</p>

<ul>
	<li>Integers in each row are sorted from left to right.</li>
	<li>The first integer of each row is greater than the last integer of the previous row.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2020/10/05/mat.jpg" style="width: 322px; height: 242px;">
<pre><strong>Input:</strong> matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 3
<strong>Output:</strong> true
</pre>

<p><strong>Example 2:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2020/10/05/mat2.jpg" style="width: 322px; height: 242px;">
<pre><strong>Input:</strong> matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 13
<strong>Output:</strong> false
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>m == matrix.length</code></li>
	<li><code>n == matrix[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 100</code></li>
	<li><code>-10<sup>4</sup> &lt;= matrix[i][j], target &lt;= 10<sup>4</sup></code></li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Binary Search](https://leetcode.com/tag/binary-search/), [Matrix](https://leetcode.com/tag/matrix/)

**Similar Questions**:
* [Search a 2D Matrix II (Medium)](https://leetcode.com/problems/search-a-2d-matrix-ii/)

## Solution 1. Binary Search

```cpp
// OJ: https://leetcode.com/problems/search-a-2d-matrix/
// Author: A M A N
// Time : O(logN * logM)
// Space: O(1)
class Solution {
public:
    int findRow(vector<vector<int>> &matrix, int target){
        int n = matrix.size();
        int m = matrix[0].size();
        int start = 0;
        int end = n-1;
        while(start<=end){
            int mid = start + (end-start)/2;
            if(matrix[mid][0]<=target and matrix[mid][m-1]>=target)
                return mid;
            else if(matrix[mid][0]<target)
                start = mid+1;
            else
                end = mid-1;
        }
        return -1;
    }
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int n = matrix.size();
        int k = findRow(matrix, target);
        if(k==-1)
            return false;
        return binary_search(matrix[k].begin(), matrix[k].end(), target);
    }
};
```


## Solution 2.

treating matrix as a sorted array

```cpp
// OJ: https://leetcode.com/problems/search-a-2d-matrix/
// Author: A M A N
// Time : O(log(N * M))
// Space: O(1)
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int n = matrix.size(), m = matrix[0].size();
        int L = 0, R = n*m-1;
        while(L<=R){
            int M = L + (R-L)/2;
            int candidate = matrix[M/m][M%m];
            if(candidate==target)
                return true;
            else if(candidate<target)
                L = M+1;
            else
                R = M-1;
        }
        return false;
    }
};
```