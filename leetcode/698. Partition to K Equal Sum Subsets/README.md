# [698. Partition to K Equal Sum Subsets (Medium)](https://leetcode.com/problems/partition-to-k-equal-sum-subsets/)

<p>Given an integer array <code>nums</code> and an integer <code>k</code>, return <code>true</code> if it is possible to divide this array into <code>k</code> non-empty subsets whose sums are all equal.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> nums = [4,3,2,3,5,2,1], k = 4
<strong>Output:</strong> true
<strong>Explanation:</strong> It's possible to divide it into 4 subsets (5), (1, 4), (2,3), (2,3) with equal sums.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> nums = [1,2,3,4], k = 3
<strong>Output:</strong> false
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= nums.length &lt;= 16</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>4</sup></code></li>
	<li>The frequency of each element is in the range <code>[1, 4]</code>.</li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/), [Backtracking](https://leetcode.com/tag/backtracking/), [Bit Manipulation](https://leetcode.com/tag/bit-manipulation/), [Memoization](https://leetcode.com/tag/memoization/), [Bitmask](https://leetcode.com/tag/bitmask/)

**Similar Questions**:
* [Partition Equal Subset Sum (Medium)](https://leetcode.com/problems/partition-equal-subset-sum/)

## Solution 1. Backtracking

```cpp
// OJ: https://leetcode.com/problems/partition-to-k-equal-sum-subsets/
// Author: A M A N
// Time: O()
// Space: O()
class Solution {
public:
    bool solve(vector<int>&nums, vector<bool> vis, int k, int idx, int curSum, int target){
        if(k == 0) return true;
        if(curSum>target)return false;
//        if found one group again start the search from the beginning of the array
        if(curSum==target) return solve(nums, vis, k-1, 0, 0, target);
        for(int i= idx; i<nums.size(); i++){
            if(!vis[i]){
                vis[i] = 1;
                if(solve(nums, vis, k, i+1, curSum+nums[i], target))
                    return true;
                vis[i] = 0;
            }
        }
        return false;
    }
    bool canPartitionKSubsets(vector<int>& nums, int k) {
        int sum = accumulate(nums.begin(), nums.end(), 0);
        if(sum % k != 0)
            return false;
        vector<bool> vis(nums.size(), false);
        return solve(nums, vis, k, 0, 0, sum/k);
    }
};
```