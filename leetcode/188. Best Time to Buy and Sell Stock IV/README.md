# [188. Best Time to Buy and Sell Stock IV (Hard)](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/)

<p>You are given an integer array <code>prices</code> where <code>prices[i]</code> is the price of a given stock on the <code>i<sup>th</sup></code> day, and an integer <code>k</code>.</p>

<p>Find the maximum profit you can achieve. You may complete at most <code>k</code> transactions.</p>

<p><strong>Note:</strong> You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> k = 2, prices = [2,4,1]
<strong>Output:</strong> 2
<strong>Explanation:</strong> Buy on day 1 (price = 2) and sell on day 2 (price = 4), profit = 4-2 = 2.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> k = 2, prices = [3,2,6,5,0,3]
<strong>Output:</strong> 7
<strong>Explanation:</strong> Buy on day 2 (price = 2) and sell on day 3 (price = 6), profit = 6-2 = 4. Then buy on day 5 (price = 0) and sell on day 6 (price = 3), profit = 3-0 = 3.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>0 &lt;= k &lt;= 100</code></li>
	<li><code>0 &lt;= prices.length &lt;= 1000</code></li>
	<li><code>0 &lt;= prices[i] &lt;= 1000</code></li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/)

**Similar Questions**:
* [Best Time to Buy and Sell Stock (Easy)](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)
* [Best Time to Buy and Sell Stock II (Easy)](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)
* [Best Time to Buy and Sell Stock III (Hard)](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/)
* 
## Solution 1. Recursion + Memoization

<p> can buy, sell(if bought), and do nothing.</p>

Choice Diagram:

![Choice Diagram](https://assets.leetcode.com/users/images/e43b8d9d-2368-45d3-9682-6f3784c479a4_1627296279.9742599.png)

```cpp
// OJ: https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/
// Author: A M A N
// Time : O(N^2*K)
// Space: O(N^2*K)
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
    int maxProfit(int k, vector<int>& prices) {
        dp.resize(2, vector<vector<int>>(k+1, vector<int>(prices.size(), -1)));
        return solve(prices, 0, k, false);    
    }
};
```