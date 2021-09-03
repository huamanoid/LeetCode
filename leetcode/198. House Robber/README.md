# [198. House Robber (Medium)](https://leetcode.com/problems/house-robber/)

<p>You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and <b>it will automatically contact the police if two adjacent houses were broken into on the same night</b>.</p>

<p>Given an integer array <code>nums</code> representing the amount of money of each house, return <em>the maximum amount of money you can rob tonight <b>without alerting the police</b></em>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> nums = [1,2,3,1]
<strong>Output:</strong> 4
<strong>Explanation:</strong> Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> nums = [2,7,9,3,1]
<strong>Output:</strong> 12
<strong>Explanation:</strong> Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1).
Total amount you can rob = 2 + 9 + 1 = 12.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>0 &lt;= nums[i] &lt;= 400</code></li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/)

**Similar Questions**:
* [Maximum Product Subarray (Medium)](https://leetcode.com/problems/maximum-product-subarray/)
* [House Robber II (Medium)](https://leetcode.com/problems/house-robber-ii/)
* [Paint House (Medium)](https://leetcode.com/problems/paint-house/)
* [Paint Fence (Medium)](https://leetcode.com/problems/paint-fence/)
* [House Robber III (Medium)](https://leetcode.com/problems/house-robber-iii/)
* [Non-negative Integers without Consecutive Ones (Hard)](https://leetcode.com/problems/non-negative-integers-without-consecutive-ones/)
* [Coin Path (Hard)](https://leetcode.com/problems/coin-path/)
* [Delete and Earn (Medium)](https://leetcode.com/problems/delete-and-earn/)

## Solution 1. DP

```cpp
// OJ: https://leetcode.com/problems/house-robber/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    int rob(vector<int>& nums) {
        int n = nums.size();
        if(n==0)return 0;
        else if(n==1)return nums[0];
        else if(n==2)return max(nums[0], nums[1]);
        vector<int> dp(1e5);
        dp[0] = nums[0];
        dp[1] = max(nums[0], nums[1]);
        for(int i=2;i<nums.size();i++){
            dp[i] = max(dp[i-1], dp[i-2]+nums[i]);
        }
        return dp[n-1];
    }
};
```