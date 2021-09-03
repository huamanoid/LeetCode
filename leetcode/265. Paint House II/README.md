# [265. Paint House II (Hard)](https://leetcode.com/problems/paint-house-ii/)

<p>There are a row of <code>n</code> houses, each house can be painted with one of the <code>k</code> colors. The cost of painting each house with a certain color is different. You have to paint all the houses such that no two adjacent houses have the same color.</p>

<p>The cost of painting each house with a certain color is represented by an <code>n x k</code> cost matrix costs.</p>

<ul>
	<li>For example, <code>costs[0][0]</code> is the cost of painting house <code>0</code> with color <code>0</code>; <code>costs[1][2]</code> is the cost of painting house <code>1</code> with color <code>2</code>, and so on...</li>
</ul>

<p>Return <em>the minimum cost to paint all houses</em>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> costs = [[1,5,3],[2,9,4]]
<strong>Output:</strong> 5
<strong>Explanation:</strong>
Paint house 0 into color 0, paint house 1 into color 2. Minimum cost: 1 + 4 = 5; 
Or paint house 0 into color 2, paint house 1 into color 0. Minimum cost: 3 + 2 = 5.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> costs = [[1,3],[2,4]]
<strong>Output:</strong> 5
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>costs.length == n</code></li>
	<li><code>costs[i].length == k</code></li>
	<li><code>1 &lt;= n &lt;= 100</code></li>
	<li><code>1 &lt;= k &lt;= 20</code></li>
	<li><code>1 &lt;= costs[i][j] &lt;= 20</code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Follow up:</strong> Could you solve it in <code>O(nk)</code> runtime?</p>


**Companies**:  
[LinkedIn](https://leetcode.com/company/linkedin)

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/)

**Similar Questions**:
* [Product of Array Except Self (Medium)](https://leetcode.com/problems/product-of-array-except-self/)
* [Sliding Window Maximum (Hard)](https://leetcode.com/problems/sliding-window-maximum/)
* [Paint House (Medium)](https://leetcode.com/problems/paint-house/)
* [Paint Fence (Medium)](https://leetcode.com/problems/paint-fence/)

## Solution 1. Recursion + Memoization

```cpp
// OJ: https://leetcode.com/problems/paint-house-ii/
// Author: A M A N
// Time : O(NKK)
// Space: O(NK)
class Solution {
public:
    unordered_map<int, unordered_map<int, int>> dp;
    int solve(vector<vector<int>>& costs, int pos, int prevCol){
        if(pos==costs.size())
            return 0;
        if(dp.find(pos)!=dp.end() and dp[pos].find(prevCol)!=dp[pos].end())
                return dp[pos][prevCol];
        int ans = INT_MAX;
        for(int i=0; i<costs[0].size(); i++){
            if(i==prevCol)continue;
            ans = min(ans, costs[pos][i] + solve(costs, pos+1, i));
        }
        return dp[pos][prevCol] = ans;
    }
    int minCostII(vector<vector<int>>& costs) {
        return solve(costs, 0, -1);
    }
};
```

## Solution 2. Dynamic Programming

```cpp
// OJ: https://leetcode.com/problems/paint-house-ii/solution/
// Author: A M A N
// Time : O(N*K^2)
// Space: O(NK)
class Solution {
public:
    int minCostII(vector<vector<int>>& costs) {
        int n = costs.size(), m = costs[0].size();
        if(n==1 and m==1) return costs[0][0];
        vector<vector<int>> dp(n+1, vector<int>(m, 1e9));
        for(int j=0; j<m; j++)dp[0][j] = 0;
        for(int i=1; i<=n; i++)
            for(int j=0; j<m; j++)
                for(int k=0; k<m; k++){
                    if(j==k)continue;
                    dp[i][j] = min(dp[i][j], costs[i-1][j] + dp[i-1][k]);
                }
        return *min_element(dp[n].begin(), dp[n].end());
    }
};
```

To optimize the runtime, Let's observe the recalculations in the above solution.
We're calculating the minimum (except the cell above it) of previous row for each cell. We can just store the minimum of prev cell and to preserve the non-adjacency condition
we can maintain a second minimum for the case when the minimum is just above the current cell.

```cpp
// OJ: https://leetcode.com/problems/paint-house-ii/solution/
// Author: A M A N
// Time : O(NK)
// Space: O(NK)
class Solution {
public:
    int minCostII(vector<vector<int>>& costs) {
        int n = costs.size(), m = costs[0].size();
        if(n==1) return *min_element(costs[0].begin(), costs[0].end());
        vector<vector<int>> dp(n+1, vector<int>(m, 1e9));
        //Base case
        for(int j=0; j<m; j++)dp[0][j] = 0;
        for(int i=1; i<=n; i++){
            int min1 = INT_MAX, min2 = INT_MAX; //minimum and second minimum of prev row
            for(int j=0; j<m; j++){
                if(min1>dp[i-1][j]){
                    min2 = min1;
                    min1 = dp[i-1][j];
                }else if(min2>dp[i-1][j])
                    min2 = dp[i-1][j];
            }
            for(int j=0; j<m; j++)
                dp[i][j] = costs[i-1][j] + (dp[i-1][j]==min1?min2:min1);
        }
        return *min_element(dp[n].begin(), dp[n].end());
    }
};
```