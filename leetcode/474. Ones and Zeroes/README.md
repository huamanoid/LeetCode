# [474. Ones and Zeroes (Medium)](https://leetcode.com/problems/ones-and-zeroes/)

<p>You are given an array of binary strings <code>strs</code> and two integers <code>m</code> and <code>n</code>.</p>

<p>Return <em>the size of the largest subset of <code>strs</code> such that there are <strong>at most</strong> </em><code>m</code><em> </em><code>0</code><em>'s and </em><code>n</code><em> </em><code>1</code><em>'s in the subset</em>.</p>

<p>A set <code>x</code> is a <strong>subset</strong> of a set <code>y</code> if all elements of <code>x</code> are also elements of <code>y</code>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> strs = ["10","0001","111001","1","0"], m = 5, n = 3
<strong>Output:</strong> 4
<strong>Explanation:</strong> The largest subset with at most 5 0's and 3 1's is {"10", "0001", "1", "0"}, so the answer is 4.
Other valid but smaller subsets include {"0001", "1"} and {"10", "1", "0"}.
{"111001"} is an invalid subset because it contains 4 1's, greater than the maximum of 3.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> strs = ["10","0","1"], m = 1, n = 1
<strong>Output:</strong> 2
<b>Explanation:</b> The largest subset is {"0", "1"}, so the answer is 2.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= strs.length &lt;= 600</code></li>
	<li><code>1 &lt;= strs[i].length &lt;= 100</code></li>
	<li><code>strs[i]</code> consists only of digits <code>'0'</code> and <code>'1'</code>.</li>
	<li><code>1 &lt;= m, n &lt;= 100</code></li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [String](https://leetcode.com/tag/string/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/)

**Similar Questions**:
* [Non-negative Integers without Consecutive Ones (Hard)](https://leetcode.com/problems/non-negative-integers-without-consecutive-ones/)

## Solution 1. Recursion + Memoization

Inspired from 0-1 Knapsack Problem

```cpp
// OJ: https://leetcode.com/problems/ones-and-zeroes/
// Author: A M A N
// Time : O(M*N*S)
// Space: O(M*N*S)
class Solution {
public:
    pair<int, int> zeroAndOne(string s){
        int z=0, o=0;
        for(char c:s)
            c=='0' ? z++ : o++;
        return {z, o};
    }
    int dp[601][101][101];
    int solve(vector<string>&strs, int i, int m, int n){
        if(i<0)return 0;
        if(m==0 and n==0) return 0;
        if(dp[i][m][n]!=-1)
            return dp[i][m][n];
        auto [z, o] = zeroAndOne(strs[i]);
        int pick = 0;
        if(m>=z and n>=o)
            pick = 1 + solve(strs, i-1, m-z, n-o); 
        int skip = solve(strs, i-1, m, n);
        return dp[i][m][n] = max(pick, skip);
    }
    int findMaxForm(vector<string>& strs, int m, int n) {
        memset(dp, -1, sizeof dp);
        return solve(strs, strs.size()-1, m, n);
    }
};
```