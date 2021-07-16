# [322. Coin Change (Medium)](https://leetcode.com/problems/coin-change/)

<p>You are given an integer array <code>coins</code> representing coins of different denominations and an integer <code>amount</code> representing a total amount of money.</p>

<p>Return <em>the fewest number of coins that you need to make up that amount</em>. If that amount of money cannot be made up by any combination of the coins, return <code>-1</code>.</p>

<p>You may assume that you have an infinite number of each kind of coin.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> coins = [1,2,5], amount = 11
<strong>Output:</strong> 3
<strong>Explanation:</strong> 11 = 5 + 5 + 1
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> coins = [2], amount = 3
<strong>Output:</strong> -1
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> coins = [1], amount = 0
<strong>Output:</strong> 0
</pre>

<p><strong>Example 4:</strong></p>

<pre><strong>Input:</strong> coins = [1], amount = 1
<strong>Output:</strong> 1
</pre>

<p><strong>Example 5:</strong></p>

<pre><strong>Input:</strong> coins = [1], amount = 2
<strong>Output:</strong> 2
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= coins.length &lt;= 12</code></li>
	<li><code>1 &lt;= coins[i] &lt;= 2<sup>31</sup> - 1</code></li>
	<li><code>0 &lt;= amount &lt;= 10<sup>4</sup></code></li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/), [Breadth-First Search](https://leetcode.com/tag/breadth-first-search/)

**Similar Questions**:
* [Minimum Cost For Tickets (Medium)](https://leetcode.com/problems/minimum-cost-for-tickets/)

## Solution 1. DP (Unbounded-Knapsack)

```cpp
// OJ: https://leetcode.com/problems/coin-change/
// Author: A M A N
// Time : O(N * M)
// Space: O(N * M)
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        int dp[coins.size()+1][amount+1];
        for(int j=0; j<=amount; j++)        dp[0][j] = 1e9;
        for(int i=0; i<=coins.size(); i++)  dp[i][0] = 0;
        for(int i=1; i<=coins.size(); i++)
            for(int j=1; j<=amount; j++)
                if(j>=coins[i-1])
                    dp[i][j] = min(dp[i-1][j], 1 + dp[i][j-coins[i-1]]);
                else
                    dp[i][j] = dp[i-1][j];
        return dp[coins.size()][amount]>=1e9? -1 : dp[coins.size()][amount];
    }
};
```

1D-DP
```cpp
// OJ: https://leetcode.com/problems/coin-change/
// Author: A M A N
// Time : O(N * M)
// Space: O(N)
class Solution {
public:
    int coinChange(vector<int>& v, int target) {
      vector<int> dp(target + 1, 1e9);
      dp[0] = 0;
      for (int i = 1; i <= target; i++) {
        for (int j = 0; j < v.size(); j++) {
          if (i - v[j] >= 0) {
            dp[i] = min(dp[i], dp[i - v[j]] + 1);
          }
        }
      }
      return (dp[target] == 1e9 ? -1 : dp[target]);
    }
};
```