# [123. Best Time to Buy and Sell Stock III (Hard)](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/)

<p>You are given an array <code>prices</code> where <code>prices[i]</code> is the price of a given stock on the <code>i<sup>th</sup></code> day.</p>

<p>Find the maximum profit you can achieve. You may complete <strong>at most two transactions</strong>.</p>

<p><strong>Note:</strong> You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> prices = [3,3,5,0,0,3,1,4]
<strong>Output:</strong> 6
<strong>Explanation:</strong> Buy on day 4 (price = 0) and sell on day 6 (price = 3), profit = 3-0 = 3.
Then buy on day 7 (price = 1) and sell on day 8 (price = 4), profit = 4-1 = 3.</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> prices = [1,2,3,4,5]
<strong>Output:</strong> 4
<strong>Explanation:</strong> Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5-1 = 4.
Note that you cannot buy on day 1, buy on day 2 and sell them later, as you are engaging multiple transactions at the same time. You must sell before buying again.
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> prices = [7,6,4,3,1]
<strong>Output:</strong> 0
<strong>Explanation:</strong> In this case, no transaction is done, i.e. max profit = 0.
</pre>

<p><strong>Example 4:</strong></p>

<pre><strong>Input:</strong> prices = [1]
<strong>Output:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= prices.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= prices[i] &lt;= 10<sup>5</sup></code></li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/)

**Similar Questions**:
* [Best Time to Buy and Sell Stock (Easy)](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)
* [Best Time to Buy and Sell Stock II (Easy)](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)
* [Best Time to Buy and Sell Stock IV (Hard)](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/)
* [Maximum Sum of 3 Non-Overlapping Subarrays (Hard)](https://leetcode.com/problems/maximum-sum-of-3-non-overlapping-subarrays/)

## Solution 1. Recursion + Memoization

<p> can buy, sell(if bought), and do nothing.</p>

Choice Diagram:

![Choice Diagram](https://assets.leetcode.com/users/images/e43b8d9d-2368-45d3-9682-6f3784c479a4_1627296279.9742599.png)

```cpp
// OJ: https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/
// Author: A M A N
// Time : O(N^2)
// Space: O(N^2)
class Solution {
public:
    vector<vector<vector<int>>> dp;
    int solve(vector<int>&prices, int pos, int transaction, bool isBought){
        if(pos==prices.size() or transaction==0)
            return 0;
        if(dp[isBought][transaction][pos]!=-1)
            return dp[isBought][transaction][pos];
        int result = solve(prices, pos+1, transaction, isBought); // Skip
        if(isBought)
            result = max(result, solve(prices, pos+1, transaction-1, false) + prices[pos]);//Sell the Share - 1 transaction is complet
        else
            result = max(result, solve(prices, pos+1, transaction, true) - prices[pos]); // Buy the share if No share is at hand
        return dp[isBought][transaction][pos] = result;
    }
    int maxProfit(vector<int>& prices) {
        dp.resize(2, vector<vector<int>>(3, vector<int>(prices.size(), -1)));
        return solve(prices, 0, 2, false);    
    }
};
```