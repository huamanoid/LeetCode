# [1547. Minimum Cost to Cut a Stick (Hard)](https://leetcode.com/problems/minimum-cost-to-cut-a-stick/)

<p>Given a wooden stick of length <code>n</code> units. The stick is labelled from <code>0</code> to <code>n</code>. For example, a stick of length <strong>6</strong> is labelled as follows:</p>
<img alt="" src="https://assets.leetcode.com/uploads/2020/07/21/statement.jpg" style="width: 521px; height: 111px;">
<p>Given an integer array <code>cuts</code>&nbsp;where <code>cuts[i]</code>&nbsp;denotes a position you should perform a cut at.</p>

<p>You should perform the cuts in order, you can change the order of the cuts as you wish.</p>

<p>The cost of one cut is the length of the stick to be cut, the total cost is the sum of costs of all cuts. When you cut a stick, it will be split into two smaller sticks (i.e. the sum of their lengths is the length of the stick before the cut). Please refer to the first example for a better explanation.</p>

<p>Return <em>the minimum total cost</em> of the&nbsp;cuts.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2020/07/23/e1.jpg" style="width: 350px; height: 284px;">
<pre><strong>Input:</strong> n = 7, cuts = [1,3,4,5]
<strong>Output:</strong> 16
<strong>Explanation:</strong> Using cuts order = [1, 3, 4, 5] as in the input leads to the following scenario:
<img alt="" src="https://assets.leetcode.com/uploads/2020/07/21/e11.jpg" style="width: 350px; height: 284px;">
The first cut is done to a rod of length 7 so the cost is 7. The second cut is done to a rod of length 6 (i.e. the second part of the first cut), the third is done to a rod of length 4 and the last cut is to a rod of length 3. The total cost is 7 + 6 + 4 + 3 = 20.
Rearranging the cuts to be [3, 5, 1, 4] for example will lead to a scenario with total cost = 16 (as shown in the example photo 7 + 4 + 3 + 2 = 16).</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> n = 9, cuts = [5,6,1,4,2]
<strong>Output:</strong> 22
<strong>Explanation:</strong> If you try the given cuts ordering the cost will be 25.
There are much ordering with total cost &lt;= 25, for example, the order [4, 6, 5, 2, 1] has total cost = 22 which is the minimum possible.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 10^6</code></li>
	<li><code>1 &lt;= cuts.length &lt;= min(n - 1, 100)</code></li>
	<li><code>1 &lt;= cuts[i] &lt;= n - 1</code></li>
	<li>All the integers in <code>cuts</code>&nbsp;array are <strong>distinct</strong>.</li>
</ul>

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/)

## Solution 1. Recursion + Memoization (Divide & Conquer)

```cpp
// OJ: https://leetcode.com/problems/minimum-cost-to-cut-a-stick/
// Author: A M A N
// Time : O(M^2) where M is the number of cuts
// Space: O(M^2)
class Solution {
public:
    unordered_map<int, unordered_map<int, int>> dp;
    unordered_set<int> sett;
    int solve(int start, int end){
        if(end <= start ) return 0;
        if(dp.find(start)!=dp.end())
            if(dp[start].find(end)!=dp[start].end())
                return dp[start][end];
        int ans = INT_MAX;
        //--------------------------------- TLE-------------------------------------------------------------------------------
        // for(int k=start+1, left, right; k<end; k++){ //  k = [start+1, end) so the cut makes the subrod of atleast length 1
        //  if(!sett.count(k)) continue;
        //--------------------------------------------------------------------------------------------------------------------
        for(auto k: sett){
            if(k<=start or k>=end) continue;    //  k = [start+1, end) so the cut makes the subrod of atleast length 1
            int left, right;
            if(dp.find(start)!=dp.end() and dp[start].find(k)!=dp[start].end()) left = dp[start][k];
            else left  = solve(start, k);
            if(dp.find(k)!=dp.end() and dp[k].find(end)!=dp[k].end()) right = dp[k][end];
            else right = solve(k, end);
            int cost  = left + right + (end - start); // length of rod as cost 
            ans = min(ans, cost); 
        }
        return dp[start][end] = (ans==INT_MAX?0:ans);    
    } 
    int minCost(int n, vector<int>& cuts) {
        for(auto a: cuts)sett.insert(a);
        return solve(0, n);
    }
};
```