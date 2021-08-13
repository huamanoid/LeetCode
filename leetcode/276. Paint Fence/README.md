# [276. Paint Fence (Medium)](https://leetcode.com/problems/paint-fence/)

<p>You are painting a fence of <code>n</code> posts with <code>k</code> different colors. You must paint the posts following these rules:</p>

<ul>
	<li>Every post must be painted <strong>exactly one</strong> color.</li>
	<li>There <strong>cannot</strong> be three or more <strong>consecutive</strong> posts with the same color.</li>
</ul>

<p>Given the two integers <code>n</code> and <code>k</code>, return <em>the <strong>number of ways</strong> you can paint the fence</em>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/02/28/paintfenceex1.png" style="width: 507px; height: 313px;">
<pre><strong>Input:</strong> n = 3, k = 2
<strong>Output:</strong> 6
<strong>Explanation: </strong>All the possibilities are shown.
Note that painting all the posts red or all the posts green is invalid because there cannot be three posts in a row with the same color.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> n = 1, k = 1
<strong>Output:</strong> 1
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> n = 7, k = 2
<strong>Output:</strong> 42
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 50</code></li>
	<li><code>1 &lt;= k &lt;= 10<sup>5</sup></code></li>
	<li>The testcases are generated such that the answer is in the range <code>[0, 2<sup>31</sup> - 1]</code> for the given <code>n</code> and <code>k</code>.</li>
</ul>


**Companies**:  
[JPMorgan](https://leetcode.com/company/jpmorgan)

**Related Topics**:  
[Dynamic Programming](https://leetcode.com/tag/dynamic-programming/)

**Similar Questions**:
* [House Robber (Medium)](https://leetcode.com/problems/house-robber/)
* [House Robber II (Medium)](https://leetcode.com/problems/house-robber-ii/)
* [Paint House (Medium)](https://leetcode.com/problems/paint-house/)
* [Paint House II (Hard)](https://leetcode.com/problems/paint-house-ii/)

## Solution 1. Dynamic Programming

```cpp
// OJ: https://leetcode.com/problems/paint-fence/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    int numWays(int n, int k) {
        if(n==1) return k;
        vector<int>dp(n); //no of ways
        dp[0] = k;
        dp[1] = k*k; // both positions can have same color
        for(int i=2; i<n; i++){
            // no of ways to paint the last fence with diff color than prev
            int noOfWaysDiff = dp[i-1]*(k-1);  //k-1 as the last paint can't be the same as prev so we have k-1 options
            // no of ways to paint the last fence with same color as prev. VALID only if prev and prevofprev are diff
            // so to paint the last with the same as prev = noOfWaysDiff at prev (to make prev and prevofprev diff) * k-1 * 1
            int noOfWaysSame = dp[i-2]*(k-1)*1; // for second last we have k-1 options and for last we have only 1 option to keep same
            dp[i] = noOfWaysDiff + noOfWaysSame;
        }
        return dp[n-1];
    }
};
```