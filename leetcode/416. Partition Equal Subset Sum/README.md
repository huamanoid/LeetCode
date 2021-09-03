# [416. Partition Equal Subset Sum (Medium)](https://leetcode.com/problems/partition-equal-subset-sum/)

<p>Given a <strong>non-empty</strong> array <code>nums</code> containing <strong>only positive integers</strong>, find if the array can be partitioned into two subsets such that the sum of elements in both subsets is equal.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> nums = [1,5,11,5]
<strong>Output:</strong> true
<strong>Explanation:</strong> The array can be partitioned as [1, 5, 5] and [11].
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> nums = [1,2,3,5]
<strong>Output:</strong> false
<strong>Explanation:</strong> The array cannot be partitioned into equal sum subsets.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 200</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 100</code></li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/)

**Similar Questions**:
* [Partition to K Equal Sum Subsets (Medium)](https://leetcode.com/problems/partition-to-k-equal-sum-subsets/)

## Solution 1. Recursion + Memoization

>Inspired from Knapsack problem

```cpp
// OJ: https://leetcode.com/problems/partition-equal-subset-sum/
// Author: A M A N
// Time : O(N * M)
// Space: O(N * M)
class Solution {
public:
    unordered_map<int, unordered_map<int, int>> dp;
    bool solve(vector<int>&nums, int i, int target){
        if(target==0)return true; // found
        if(i==nums.size())return false;
        if(dp.find(i)!=dp.end() and dp[i].find(target)!=dp[i].end())
            return dp[i][target];
        if(nums[i] <= target)
            if(solve(nums, i+1, target-nums[i])) //pick
                return dp[i][target] = true; 
        return dp[i][target] = solve(nums, i+1, target); //skip
    }
    bool canPartition(vector<int>& nums) {
        int sum = accumulate(nums.begin(), nums.end(), 0);
        if(sum&1)return false; // Sum is odd
        return solve(nums,0, sum/2);
    }
};
```

## Solution 2. DP

```cpp
// OJ: https://leetcode.com/problems/partition-equal-subset-sum/
// Author: A M A N
// Time : O(N * M) where M is targetSum
// Space: O(N * M)
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int n = nums.size();
        int sum = accumulate(nums.begin(), nums.end(), 0);
        if(sum&1)return false; // Sum is odd
        int target = sum/2;
        vector<vector<bool>> dp(n+1, vector<bool>(target+1, false));
        for(int i=0; i<=n; i++) dp[i][0] = true; // Base Case: Always possible to make subset of sum 0
        for(int i=1; i<=n; i++)
            for(int j=1; j<=target; j++)
                 // exclude or include (if possible)
                if(nums[i-1] <= j) 
                    dp[i][j] = dp[i-1][j] or dp[i-1][j-nums[i-1]];
                else
                    dp[i][j] = dp[i-1][j];
        return dp[n][target];
    }
};
```