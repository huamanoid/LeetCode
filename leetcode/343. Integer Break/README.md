# [343. Integer Break (Medium)](https://leetcode.com/problems/integer-break/)

<p>Given an integer <code>n</code>, break it into the sum of <code>k</code> <strong>positive integers</strong>, where <code>k &gt;= 2</code>, and maximize the product of those integers.</p>

<p>Return <em>the maximum product you can get</em>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> n = 2
<strong>Output:</strong> 1
<strong>Explanation:</strong> 2 = 1 + 1, 1 × 1 = 1.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> n = 10
<strong>Output:</strong> 36
<strong>Explanation:</strong> 10 = 3 + 3 + 4, 3 × 3 × 4 = 36.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 58</code></li>
</ul>


**Companies**:  
[Citadel](https://leetcode.com/company/citadel), [Google](https://leetcode.com/company/google), [Bloomberg](https://leetcode.com/company/bloomberg), [Apple](https://leetcode.com/company/apple)

**Related Topics**:  
[Math](https://leetcode.com/tag/math/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/)

**Similar Questions**:
* [Maximize Number of Nice Divisors (Hard)](https://leetcode.com/problems/maximize-number-of-nice-divisors/)

## Solution 1. DFS

```cpp
// OJ: https://leetcode.com/problems/integer-break/
// Author: A M A N
// Time : O(N^2)
// Space: O(N)
class Solution {
public:
    int dp[60];
    int solve(int n){
        if(n==1) return 1;
        if(n==2) return 1;
        if(dp[n])
            return dp[n];
        int ans = -1e9;
        for(int i=1; i<n; i++){
            int a = i*(n-i); // do not break (n-i) further
            int b = i*solve(n-i); // break (n-i) further
            ans = max({ans, a, b});
        }
        return dp[n] = ans;
    }
    int integerBreak(int n) {
        return  solve(n);
    }
};
```