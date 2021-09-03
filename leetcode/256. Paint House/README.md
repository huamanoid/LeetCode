# [256. Paint House (Medium)](https://leetcode.com/problems/paint-house/)

<p>There is a row of <code>n</code> houses, where each house can be painted one of three colors: red, blue, or green. The cost of painting each house with a certain color is different. You have to paint all the houses such that no two adjacent houses have the same color.</p>

<p>The cost of painting each house with a certain color is represented by an <code>n x 3</code> cost matrix <code>costs</code>.</p>

<ul>
	<li>For example, <code>costs[0][0]</code> is the cost of painting house <code>0</code> with the color red; <code>costs[1][2]</code> is the cost of painting house 1 with color green, and so on...</li>
</ul>

<p>Return <em>the minimum cost to paint all houses</em>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> costs = [[17,2,17],[16,16,5],[14,3,19]]
<strong>Output:</strong> 10
<strong>Explanation:</strong> Paint house 0 into blue, paint house 1 into green, paint house 2 into blue.
Minimum cost: 2 + 5 + 3 = 10.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> costs = [[7,6,2]]
<strong>Output:</strong> 2
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>costs.length == n</code></li>
	<li><code>costs[i].length == 3</code></li>
	<li><code>1 &lt;= n &lt;= 100</code></li>
	<li><code>1 &lt;= costs[i][j] &lt;= 20</code></li>
</ul>


**Companies**:  
[LinkedIn](https://leetcode.com/company/linkedin)

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/)

**Similar Questions**:
* [House Robber (Medium)](https://leetcode.com/problems/house-robber/)
* [House Robber II (Medium)](https://leetcode.com/problems/house-robber-ii/)
* [Paint House II (Hard)](https://leetcode.com/problems/paint-house-ii/)
* [Paint Fence (Medium)](https://leetcode.com/problems/paint-fence/)

## Solution 1. Recursion + Memoization

```cpp
// OJ: https://leetcode.com/problems/paint-house/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    unordered_map<int, unordered_map<int, int>> dp;
    int solve(vector<vector<int>>& costs, int pos, int prevCol){
        if(pos==costs.size())
            return 0;
        if(dp.find(pos)!=dp.end() and dp[pos].find(prevCol)!=dp[pos].end())
                return dp[pos][prevCol];
        int ans = INT_MAX;
        for(int i=0; i<3; i++){
            if(i==prevCol)continue;
            ans = min(ans, costs[pos][i] + solve(costs, pos+1, i));
        }
        return dp[pos][prevCol] = ans;
    }
    int minCost(vector<vector<int>>& costs) {
        return solve(costs, 0, -1);
    }
};
```


## Solution 2. Dynamic Programming

```cpp
// OJ: https://leetcode.com/problems/paint-house/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    int minCost(vector<vector<int>>& costs) {
        int n = costs.size();
        vector<vector<int>> dp(3, vector<int>(n+1, 1e9));
        dp[0][0] = dp[1][0] = dp[2][0] = 0;
        for(int j=1; j<=n; j++){
            for(int i=0; i<3; i++){
                for(int k=0; k<3; k++){
                    if(i==k)continue;
                    dp[i][j] = min(dp[i][j], costs[j-1][i] + dp[k][j-1]);
                }
            }
        }
        return min({dp[0][n], dp[1][n], dp[2][n]});
    }
};
```