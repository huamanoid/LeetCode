# [910. Smallest Range II (Medium)](https://leetcode.com/problems/smallest-range-ii/)

<p>You are given an integer array <code>nums</code> and an integer <code>k</code>.</p>

<p>For each index <code>i</code> where <code>0 &lt;= i &lt; nums.length</code>, change <code>nums[i]</code> to be either <code>nums[i] + k</code> or <code>nums[i] - k</code>.</p>

<p>The <strong>score</strong> of <code>nums</code> is the difference between the maximum and minimum elements in <code>nums</code>.</p>

<p>Return <em>the minimum <strong>score</strong> of </em><code>nums</code><em> after changing the values at each index</em>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> nums = [1], k = 0
<strong>Output:</strong> 0
<strong>Explanation:</strong> The score is max(nums) - min(nums) = 1 - 1 = 0.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> nums = [0,10], k = 2
<strong>Output:</strong> 6
<strong>Explanation:</strong> Change nums to be [2, 8]. The score is max(nums) - min(nums) = 8 - 2 = 6.
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> nums = [1,3,6], k = 3
<strong>Output:</strong> 3
<strong>Explanation:</strong> Change nums to be [4, 6, 3]. The score is max(nums) - min(nums) = 6 - 3 = 3.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= k &lt;= 10<sup>4</sup></code></li>
</ul>


**Companies**:  
[Adobe](https://leetcode.com/company/adobe)

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Math](https://leetcode.com/tag/math/), [Greedy](https://leetcode.com/tag/greedy/), [Sorting](https://leetcode.com/tag/sorting/)

## Solution 1. Recursion + Memoization 

> TLE

```cpp
// OJ: https://leetcode.com/problems/smallest-range-ii/
// Author: A M A N
// Time : O()
// Space: O()
class Solution {
public:
    map<array<int, 3>, int> dp;
    int solve(vector<int>&nums, int mn, int mx, int idx, int k){
        if(idx>=nums.size()) return mx-mn;
        if(dp.find({mn, mx, idx})!=dp.end())
            return dp[{mn, mx, idx}];
        int A = solve(nums, min(mn, nums[idx]+k), max(mx, nums[idx]+k), idx+1, k);
        int B = solve(nums, min(mn, nums[idx]-k), max(mx, nums[idx]-k), idx+1, k);
        return dp[{mn, mx, idx}] = min(A, B);
    }
    int smallestRangeII(vector<int>& nums, int k) {
        return solve(nums, 1e9, -1e9, 0, k);
    }
};
```

## Solution 2. Greedy

```cpp
// OJ: https://leetcode.com/problems/smallest-range-ii/
// Author: A M A N
// Time : O(NlogN)
// Space: O(1)
class Solution {
public:
    int smallestRangeII(vector<int>& nums, int k) {
        int n = nums.size();
        sort(nums.begin(), nums.end());
        int ans = nums[n-1] - nums[0];
        for(int i=0; i<n-1; i++){
            int small = nums[i], big = nums[i+1]; // minimize the bigger and maximize the smaller
            int mx = max(nums[n-1]-k, small+k); // small+k is already bigger than nums[0]+k
            int mn = min(nums[0]+k, big-k);     // big -k is already smaller than nums[n-1]-k
            ans = min(ans, mx-mn);
        }
        return ans;
    }
};
```