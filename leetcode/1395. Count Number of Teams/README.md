# [1395. Count Number of Teams (Medium)](https://leetcode.com/problems/count-number-of-teams/)

<p>There are <code>n</code> soldiers standing in a line. Each soldier is assigned a <strong>unique</strong> <code>rating</code> value.</p>

<p>You have to form a team of 3 soldiers amongst them under the following rules:</p>

<ul>
	<li>Choose 3 soldiers with index (<code>i</code>, <code>j</code>, <code>k</code>) with rating (<code>rating[i]</code>, <code>rating[j]</code>, <code>rating[k]</code>).</li>
	<li>A team is valid if: (<code>rating[i] &lt; rating[j] &lt; rating[k]</code>) or (<code>rating[i] &gt; rating[j] &gt; rating[k]</code>) where (<code>0 &lt;= i &lt; j &lt; k &lt; n</code>).</li>
</ul>

<p>Return the number of teams you can form given the conditions. (soldiers can be part of multiple teams).</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> rating = [2,5,3,4,1]
<strong>Output:</strong> 3
<strong>Explanation:</strong> We can form three teams given the conditions. (2,3,4), (5,4,1), (5,3,1). 
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> rating = [2,1,3]
<strong>Output:</strong> 0
<strong>Explanation:</strong> We can't form any team given the conditions.
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> rating = [1,2,3,4]
<strong>Output:</strong> 4
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>n == rating.length</code></li>
	<li><code>3 &lt;= n &lt;= 1000</code></li>
	<li><code>1 &lt;= rating[i] &lt;= 10<sup>5</sup></code></li>
	<li>All the integers in <code>rating</code> are <strong>unique</strong>.</li>
</ul>


**Companies**:  
[Goldman Sachs](https://leetcode.com/company/goldman-sachs), [Amazon](https://leetcode.com/company/amazon), [Apple](https://leetcode.com/company/apple)

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/), [Binary Indexed Tree](https://leetcode.com/tag/binary-indexed-tree/)

## Solution 1.

```cpp
// OJ: https://leetcode.com/problems/count-number-of-teams/
// Author: A M A N
// Time : O(N^2)
// Space: O(1)
class Solution {
public:
    int numTeams(vector<int>& rating) {
        int n = rating.size();
        int ans = 0;
        for(int j=0; j<n; j++){
            int leftLess = 0, leftMore = 0, rightLess = 0, rightMore = 0;
            for(int i=0; i<j; i++)
                if(rating[i]<rating[j]) 
                    leftLess++;
                else if(rating[i]>rating[j])
                    leftMore++;
            for(int k=j+1; k<n; k++)
                if(rating[j]<rating[k])
                    rightMore++;
                else if(rating[j]>rating[k])
                    rightLess++;
            ans += leftLess*rightMore + leftMore*rightLess;
        }
        return ans;
    }
};
```