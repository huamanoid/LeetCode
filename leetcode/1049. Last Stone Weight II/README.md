# [1049. Last Stone Weight II (Medium)](https://leetcode.com/problems/last-stone-weight-ii/)

<p>You are given an array of integers <code>stones</code> where <code>stones[i]</code> is the weight of the <code>i<sup>th</sup></code> stone.</p>

<p>We are playing a game with the stones. On each turn, we choose any two stones and smash them together. Suppose the stones have weights <code>x</code> and <code>y</code> with <code>x &lt;= y</code>. The result of this smash is:</p>

<ul>
	<li>If <code>x == y</code>, both stones are destroyed, and</li>
	<li>If <code>x != y</code>, the stone of weight <code>x</code> is destroyed, and the stone of weight <code>y</code> has new weight <code>y - x</code>.</li>
</ul>

<p>At the end of the game, there is <strong>at most one</strong> stone left.</p>

<p>Return <em>the smallest possible weight of the left stone</em>. If there are no stones left, return <code>0</code>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> stones = [2,7,4,1,8,1]
<strong>Output:</strong> 1
<strong>Explanation:</strong>
We can combine 2 and 4 to get 2, so the array converts to [2,7,1,8,1] then,
we can combine 7 and 8 to get 1, so the array converts to [2,1,1,1] then,
we can combine 2 and 1 to get 1, so the array converts to [1,1,1] then,
we can combine 1 and 1 to get 0, so the array converts to [1], then that's the optimal value.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> stones = [31,26,33,21,40]
<strong>Output:</strong> 5
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> stones = [1,2]
<strong>Output:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= stones.length &lt;= 30</code></li>
	<li><code>1 &lt;= stones[i] &lt;= 100</code></li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/)

## Solution 1. DP 

`Minimum subsetsum difference`

```cpp
// OJ: https://leetcode.com/problems/last-stone-weight-ii/
// Author: A M A N
// Time : O(N*S) where S is the sum of elements of N
// Space: O(N*S)
// Divide & Conquer (MCM) not applicable because optimal left subprolem and optimal right subproblem doesnt give an optimum solution 

class Solution {
public:
    int lastStoneWeightII(vector<int>& stones){
        int n = stones.size();
        int sum = accumulate(stones.begin(), stones.end(), 0); 
        bool dp[n+1][sum+1];
        for(int j=0; j<=sum; j++) dp[0][j] = false; // cant sum nothing to any number
        for(int i=0; i<=n; i++)   dp[i][0] = true; // always possible to make sum zero
        for(int i=1; i<=n; i++)
            for(int j=1; j<=sum; j++)
                if(j>=stones[i-1])
                    dp[i][j] = dp[i-1][j] or dp[i-1][j-stones[i-1]]; // skip or pick
                else
                    dp[i][j] = dp[i-1][j]; // skip if can't pick
        
        for(int j=sum/2; j<=sum; j++)
            if(dp[n][j])
                return abs(2*j - sum);
        return -1;                
    }
};
```