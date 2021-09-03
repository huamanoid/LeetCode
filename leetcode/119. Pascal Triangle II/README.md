# [119. Pascal's Triangle II (Easy)](https://leetcode.com/problems/pascals-triangle-ii/)

<p>Given an integer <code>rowIndex</code>, return the <code>rowIndex<sup>th</sup></code> (<strong>0-indexed</strong>) row of the <strong>Pascal's triangle</strong>.</p>

<p>In <strong>Pascal's triangle</strong>, each number is the sum of the two numbers directly above it as shown:</p>
<img alt="" src="https://upload.wikimedia.org/wikipedia/commons/0/0d/PascalTriangleAnimated2.gif" style="height:240px; width:260px">
<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<pre><strong>Input:</strong> rowIndex = 3
<strong>Output:</strong> [1,3,3,1]
</pre><p><strong>Example 2:</strong></p>
<pre><strong>Input:</strong> rowIndex = 0
<strong>Output:</strong> [1]
</pre><p><strong>Example 3:</strong></p>
<pre><strong>Input:</strong> rowIndex = 1
<strong>Output:</strong> [1,1]
</pre>
<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>0 &lt;= rowIndex &lt;= 33</code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Follow up:</strong> Could you optimize your algorithm to use only <code>O(rowIndex)</code> extra space?</p>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/)

**Similar Questions**:
* [Pascal's Triangle (Easy)](https://leetcode.com/problems/pascals-triangle/)

## Solution 1. Recursion

```cpp
// OJ: https://leetcode.com/problems/pascals-triangle-ii/
// Author: A M A N
// Time : O(N^2)
// Space: O(N)
class Solution {
public:
    vector<int> getRow(int row) {
        if(row==0)
            return {1};
        vector<int> prev = getRow(row-1);
        vector<int> cur(prev.size()+1);
        for(int i=0; i<cur.size(); i++)
            if(i==0 or i==cur.size()-1)
                cur[i] = 1;
            else
                cur[i] = prev[i] + prev[i-1];
        return cur;
    }
};
```