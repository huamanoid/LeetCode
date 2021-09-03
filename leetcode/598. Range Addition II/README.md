# [598. Range Addition II (Easy)](https://leetcode.com/problems/range-addition-ii/)

<p>You are given an <code>m x n</code> matrix <code>M</code> initialized with all <code>0</code>'s and an array of operations <code>ops</code>, where <code>ops[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> means <code>M[x][y]</code> should be incremented by one for all <code>0 &lt;= x &lt; a<sub>i</sub></code> and <code>0 &lt;= y &lt; b<sub>i</sub></code>.</p>

<p>Count and return <em>the number of maximum integers in the matrix after performing all the operations</em>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2020/10/02/ex1.jpg" style="width: 750px; height: 176px;">
<pre><strong>Input:</strong> m = 3, n = 3, ops = [[2,2],[3,3]]
<strong>Output:</strong> 4
<strong>Explanation:</strong> The maximum integer in M is 2, and there are four of it in M. So return 4.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> m = 3, n = 3, ops = [[2,2],[3,3],[3,3],[3,3],[2,2],[3,3],[3,3],[3,3],[2,2],[3,3],[3,3],[3,3]]
<strong>Output:</strong> 4
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> m = 3, n = 3, ops = []
<strong>Output:</strong> 9
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= m, n &lt;= 4 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= ops.length &lt;= 10<sup>4</sup></code></li>
	<li><code>ops[i].length == 2</code></li>
	<li><code>1 &lt;= a<sub>i</sub> &lt;= m</code></li>
	<li><code>1 &lt;= b<sub>i</sub> &lt;= n</code></li>
</ul>


**Companies**:  
[IXL](https://leetcode.com/company/ixl)

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Math](https://leetcode.com/tag/math/)

**Similar Questions**:
* [Range Addition (Medium)](https://leetcode.com/problems/range-addition/)

## Solution 1.

The maximum element will be the one on which all the operations have been performed

```cpp
// OJ: https://leetcode.com/problems/range-addition-ii/
// Author: A M A N
// Time : O(K)
// Space: O(1)
class Solution {
public:
    int maxCount(int m, int n, vector<vector<int>>& ops) {
        int minX = m, minY = n;
        for(auto v: ops){
            minX = min(minX, v[0]);
            minY = min(minY, v[1]);
        }
        return minX*minY;
    }
};
```