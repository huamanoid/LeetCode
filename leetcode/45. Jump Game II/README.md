# [45. Jump Game II (Medium)](https://leetcode.com/problems/jump-game-ii/)

<p>Given an array of non-negative integers <code>nums</code>, you are initially positioned at the first index of the array.</p>

<p>Each element in the array represents your maximum jump length at that position.</p>

<p>Your goal is to reach the last index in the minimum number of jumps.</p>

<p>You can assume that you can always reach the last index.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> nums = [2,3,1,1,4]
<strong>Output:</strong> 2
<strong>Explanation:</strong> The minimum number of jumps to reach the last index is 2. Jump 1 step from index 0 to 1, then 3 steps to the last index.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> nums = [2,3,0,1,4]
<strong>Output:</strong> 2
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 1000</code></li>
</ul>


**Companies**:  
[Amazon](https://leetcode.com/company/amazon), [Apple](https://leetcode.com/company/apple), [Snapchat](https://leetcode.com/company/snapchat), [Google](https://leetcode.com/company/google), [Yahoo](https://leetcode.com/company/yahoo), [Flipkart](https://leetcode.com/company/flipkart), [tcs](https://leetcode.com/company/tcs)

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/), [Greedy](https://leetcode.com/tag/greedy/)

**Similar Questions**:
* [Jump Game (Medium)](https://leetcode.com/problems/jump-game/)
* [Jump Game III (Medium)](https://leetcode.com/problems/jump-game-iii/)
* [Jump Game VII (Medium)](https://leetcode.com/problems/jump-game-vii/)

## Solution 1. Recursion + Memoization

```cpp
// OJ: https://leetcode.com/problems/jump-game-ii/
// Author: A M A N
// Time : O(N * M) where M is the max jump possible
// Space: O(N)
class Solution {
public:
    int dp[10001];
    int solve(vector<int>&nums, int idx){
        if(idx>=nums.size()-1)return 0;
        if(nums[idx]==0) return 1e9;
        if(dp[idx]!=-1) return dp[idx];
        int ans = 1e9;
        for(int i=idx+1; i<=idx+nums[idx]; i++)
            ans = min(ans, 1+solve(nums, i));
        return dp[idx] = ans;
    }
    int jump(vector<int>& nums) {
        memset(dp, -1, sizeof dp);
        return solve(nums, 0);
    }
};
```