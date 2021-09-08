# [279. Perfect Squares (Medium)](https://leetcode.com/problems/perfect-squares/)

<p>Given an integer <code>n</code>, return <em>the least number of perfect square numbers that sum to</em> <code>n</code>.</p>

<p>A <strong>perfect square</strong> is an integer that is the square of an integer; in other words, it is the product of some integer with itself. For example, <code>1</code>, <code>4</code>, <code>9</code>, and <code>16</code> are perfect squares while <code>3</code> and <code>11</code> are not.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> n = 12
<strong>Output:</strong> 3
<strong>Explanation:</strong> 12 = 4 + 4 + 4.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> n = 13
<strong>Output:</strong> 2
<strong>Explanation:</strong> 13 = 4 + 9.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>4</sup></code></li>
</ul>


**Companies**:  
[Amazon](https://leetcode.com/company/amazon), [eBay](https://leetcode.com/company/ebay)

**Related Topics**:  
[Math](https://leetcode.com/tag/math/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/), [Breadth-First Search](https://leetcode.com/tag/breadth-first-search/)

**Similar Questions**:
* [Count Primes (Easy)](https://leetcode.com/problems/count-primes/)
* [Ugly Number II (Medium)](https://leetcode.com/problems/ugly-number-ii/)

## Solution 1. Bottom-Up DP
 
```cpp
// OJ: https://leetcode.com/problems/perfect-squares/
// Author: A M A N
// Time : O(N*sqrt(N)) where S is the no of Squre numbers less than N
// Space: O(N)
class Solution {
public:
    unordered_map<int, int> dp;
    int solve(int n){
        if(n==0) return 0;
        if(dp.find(n)!=dp.end()) return dp[n];
        int ans = 1e9;
        for(int i=1; i*i<=n; i++)
            ans = min(ans, 1+solve(n-i*i));
        return dp[n] = ans;
    }
    int numSquares(int n) {
        return solve(n);
    }
};
```