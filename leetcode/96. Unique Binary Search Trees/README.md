# [96. Unique Binary Search Trees (Medium)](https://leetcode.com/problems/unique-binary-search-trees/)

<p>Given an integer <code>n</code>, return <em>the number of structurally unique <strong>BST'</strong>s (binary search trees) which has exactly </em><code>n</code><em> nodes of unique values from</em> <code>1</code> <em>to</em> <code>n</code>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/01/18/uniquebstn3.jpg" style="width: 600px; height: 148px;">
<pre><strong>Input:</strong> n = 3
<strong>Output:</strong> 5
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> n = 1
<strong>Output:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 19</code></li>
</ul>


**Related Topics**:  
[Math](https://leetcode.com/tag/math/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/), [Tree](https://leetcode.com/tag/tree/), [Binary Search Tree](https://leetcode.com/tag/binary-search-tree/), [Binary Tree](https://leetcode.com/tag/binary-tree/)

**Similar Questions**:
* [Unique Binary Search Trees II (Medium)](https://leetcode.com/problems/unique-binary-search-trees-ii/)

## Solution 1. Recursion - Catalan number

```cpp
// OJ: https://leetcode.com/problems/unique-binary-search-trees/
// Author: A M A N
// Time : O(N)
// Space: O(N^2) can be reduced to O(N) if we check before calling
class Solution {
public:
    int dp[20];
    int numTrees(int n) {
        if(n<2) return 1;
        if(dp[n]) return dp[n];
        int ans =0;
        for(int i=0; i<n; i++)
            ans += numTrees(i)*numTrees(n-1-i);
        return dp[n]=ans;
    }
};
```

## Solution 2. DP - Catalan number

```cpp
// OJ: https://leetcode.com/problems/unique-binary-search-trees/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    int numTrees(int n){
        int dp[n+1];
        memset(dp, 0, sizeof dp);
        dp[0] = dp[1] = 1;
        for(int i=2; i<=n; i++)
            for(int j=0; j<i; j++)
                dp[i] += dp[j]*dp[i-1-j];                
        return dp[n];
    }
};
```