# [55. Jump Game (Medium)](https://leetcode.com/problems/jump-game/)

<p>You are given an integer array <code>nums</code>. You are initially positioned at the array's <strong>first index</strong>, and each element in the array represents your maximum jump length at that position.</p>

<p>Return <code>true</code><em> if you can reach the last index, or </em><code>false</code><em> otherwise</em>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> nums = [2,3,1,1,4]
<strong>Output:</strong> true
<strong>Explanation:</strong> Jump 1 step from index 0 to 1, then 3 steps to the last index.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> nums = [3,2,1,0,4]
<strong>Output:</strong> false
<strong>Explanation:</strong> You will always arrive at index 3 no matter what. Its maximum jump length is 0, which makes it impossible to reach the last index.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>


**Companies**:  
[Amazon](https://leetcode.com/company/amazon), [Uber](https://leetcode.com/company/uber), [Apple](https://leetcode.com/company/apple), [Microsoft](https://leetcode.com/company/microsoft), [Facebook](https://leetcode.com/company/facebook), [ByteDance](https://leetcode.com/company/bytedance), [Goldman Sachs](https://leetcode.com/company/goldman-sachs)

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/), [Greedy](https://leetcode.com/tag/greedy/)

**Similar Questions**:
* [Jump Game II (Medium)](https://leetcode.com/problems/jump-game-ii/)
* [Jump Game III (Medium)](https://leetcode.com/problems/jump-game-iii/)
* [Jump Game VII (Medium)](https://leetcode.com/problems/jump-game-vii/)

## Solution 1. Recursion + Memoization

```cpp
// OJ: https://leetcode.com/problems/jump-game/
// Author: A M A N
// Time : O(N^2)
// Space: O(N)
class Solution {
public:
    int dp[10001];
    bool solve(vector<int>&nums, int i){
        if(i>=nums.size()-1) return true;
        if(dp[i]!=-1) 
            return dp[i];
        bool can = false;
        if(nums[i])
            for(int j=i+1; j<=i+nums[i] and !can; j++)
                can |= solve(nums, j);
        return dp[i] = can;
    }
    bool canJump(vector<int>& nums) {
        memset(dp, -1, sizeof dp);
        return solve(nums, 0);
    }
};
```


## Solution 2. Greedy

```cpp
// OJ: https://leetcode.com/problems/jump-game/
// Author: A M A N
// Time : O(N)
// Space: O(1)
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int canGoUpto = 0;
        for(int i=0; i<nums.size(); i++){
            if(canGoUpto>=nums.size()-1) return true;
            canGoUpto = max(canGoUpto, i+nums[i]);
            if(i==canGoUpto) return false;
        }
        return -1;
    }
};
```