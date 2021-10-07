# [1798. Maximum Number of Consecutive Values You Can Make (Medium)](https://leetcode.com/problems/maximum-number-of-consecutive-values-you-can-make/)

<p>You are given an integer array <code>coins</code> of length <code>n</code> which represents the <code>n</code> coins that you own. The value of the <code>i<sup>th</sup></code> coin is <code>coins[i]</code>. You can <strong>make</strong> some value <code>x</code> if you can choose some of your <code>n</code> coins such that their values sum up to <code>x</code>.</p>

<p>Return the <em>maximum number of consecutive integer values that you <strong>can</strong> <strong>make</strong> with your coins <strong>starting</strong> from and <strong>including</strong> </em><code>0</code>.</p>

<p>Note that you may have multiple coins of the same value.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> coins = [1,3]
<strong>Output:</strong> 2
<strong>Explanation: </strong>You can make the following values:
- 0: take []
- 1: take [1]
You can make 2 consecutive integer values starting from 0.</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> coins = [1,1,1,4]
<strong>Output:</strong> 8
<strong>Explanation: </strong>You can make the following values:
- 0: take []
- 1: take [1]
- 2: take [1,1]
- 3: take [1,1,1]
- 4: take [4]
- 5: take [4,1]
- 6: take [4,1,1]
- 7: take [4,1,1,1]
You can make 8 consecutive integer values starting from 0.</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> nums = [1,4,10,3,1]
<strong>Output:</strong> 20</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>coins.length == n</code></li>
	<li><code>1 &lt;= n &lt;= 4 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= coins[i] &lt;= 4 * 10<sup>4</sup></code></li>
</ul>


**Companies**:  
[Infosys](https://leetcode.com/company/infosys)

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Greedy](https://leetcode.com/tag/greedy/)

**Similar Questions**:
* [Patching Array (Hard)](https://leetcode.com/problems/patching-array/)

## Solution 1. Using 0/1 Knapsack problem


>TLE

```cpp
// OJ: https://leetcode.com/problems/maximum-number-of-consecutive-values-you-can-make/
// Author: A M A N
// Time : O(N*M) where M is sum(coins)
// Space: O(N*M)
class Solution {
public:
    int getMaximumConsecutive(vector<int>& coins) {
        int n = coins.size();
        long m = accumulate(coins.begin(), coins.end(), 0LL);
        //------------------0/1 - knapsack
        vector<vector<bool>> dp(n+1, vector<bool>(m, false));
        for(int i=0; i<=n; i++)
            dp[i][0] = true;
        for(int i=1; i<=n; i++)
            for(long j=1; j<=m; j++){
                if(coins[i-1]<=j)
                    dp[i][j] = dp[i-1][j] or dp[i-1][j-coins[i-1]];
                else
                    dp[i][j] = dp[i-1][j];
            }
        //-------------------
        for(long j=0; j<=m; j++)
            if(!dp[n][j])
                return j;
        return m+1;
    }
};
```


## Solution 2. Sorting and Greedy

Assume we can form [0, cnt) numbers and now we get a new number k, then we can form [k, cnt + k) numbers. As long as k <= cnt, we can extend the range to [0, cnt + k).

```cpp
// OJ: https://leetcode.com/problems/maximum-number-of-consecutive-values-you-can-make/
// Author: A M A N
// Time : O(NlogN)
// Space: O(1)
class Solution {
public:
    int getMaximumConsecutive(vector<int>& coins) {
        sort(coins.begin(), coins.end());
        int cnt = 1; 
        // cnt => 1 + total sum of all coins seen so far
        for(int n: coins){
            // if the current coin value is more than `cnt`
            // which means the coin values [cnt:n] can't be formed
            if(n > cnt) break; 
            cnt + =n;
        }
        return cnt;
    }
};
```