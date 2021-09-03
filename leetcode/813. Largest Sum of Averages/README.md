# [813. Largest Sum of Averages (Medium)](https://leetcode.com/problems/largest-sum-of-averages/)

<p>You are given an integer array <code>nums</code> and an integer <code>k</code>. You can partition the array into <strong>at most</strong> <code>k</code> non-empty adjacent subarrays. The <strong>score</strong> of a partition is the sum of the averages of each subarray.</p>

<p>Note that the partition must use every integer in <code>nums</code>, and that the score is not necessarily an integer.</p>

<p>Return <em>the maximum <strong>score</strong> you can achieve of all the possible partitions</em>. Answers within <code>10<sup>-6</sup></code> of the actual answer will be accepted.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> nums = [9,1,2,3,9], k = 3
<strong>Output:</strong> 20.00000
<strong>Explanation:</strong> 
The best choice is to partition nums into [9], [1, 2, 3], [9]. The answer is 9 + (1 + 2 + 3) / 3 + 9 = 20.
We could have also partitioned nums into [9, 1], [2], [3, 9], for example.
That partition would lead to a score of 5 + 2 + 6 = 13, which is worse.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> nums = [1,2,3,4,5,6,7], k = 4
<strong>Output:</strong> 20.50000
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= k &lt;= nums.length</code></li>
</ul>


**Companies**:  
[Adobe](https://leetcode.com/company/adobe)

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/)

## Solution 1. Recursion + Memoization

```cpp
// OJ: https://leetcode.com/problems/largest-sum-of-averages/
// Author: A M A N
// Time : O(N^3)
// Space: O(N^2)
class Solution {
public:
    int sum[101];
    double dp[101][101];
    double averageInRange(vector<int>&nums, int i, int j){
        return ((double)sum[j]-sum[i]+nums[i])/(j-i+1);
    }
    double solve(vector<int>&nums, int n, int start, int k){
        if(dp[start][k]!=0)
            return dp[start][k];
        if(k==1) 
            return dp[start][k] = averageInRange(nums, start, n-1);
        double ans = 0;
        for(int i=start; i+k<=n; i++)
            ans =  max(ans, averageInRange(nums, start, i) + solve(nums, n, i+1, k-1));
        return dp[start][k] = ans;
    }
    double largestSumOfAverages(vector<int>& nums, int k) {
        memset(dp, 0, sizeof dp);
        for(int i=0; i<nums.size(); i++)
            sum[i] = nums[i] + (i?sum[i-1]:0);
        return solve(nums, nums.size(), 0, k);
    }
};
```