# [376. Wiggle Subsequence (Medium)](https://leetcode.com/problems/wiggle-subsequence/)

<p>A <strong>wiggle sequence</strong> is a sequence where the differences between successive numbers strictly alternate between positive and negative. The first difference (if one exists) may be either positive or negative. A sequence with one element and a sequence with two non-equal elements are trivially wiggle sequences.</p>

<ul>
	<li>For example, <code>[1, 7, 4, 9, 2, 5]</code> is a <strong>wiggle sequence</strong> because the differences <code>(6, -3, 5, -7, 3)</code> alternate between positive and negative.</li>
	<li>In contrast, <code>[1, 4, 7, 2, 5]</code> and <code>[1, 7, 4, 5, 5]</code> are not wiggle sequences. The first is not because its first two differences are positive, and the second is not because its last difference is zero.</li>
</ul>

<p>A <strong>subsequence</strong> is obtained by deleting some elements (possibly zero) from the original sequence, leaving the remaining elements in their original order.</p>

<p>Given an integer array <code>nums</code>, return <em>the length of the longest <strong>wiggle subsequence</strong> of </em><code>nums</code>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> nums = [1,7,4,9,2,5]
<strong>Output:</strong> 6
<strong>Explanation:</strong> The entire sequence is a wiggle sequence with differences (6, -3, 5, -7, 3).
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> nums = [1,17,5,10,13,15,10,5,16,8]
<strong>Output:</strong> 7
<strong>Explanation:</strong> There are several subsequences that achieve this length.
One is [1, 17, 10, 13, 10, 16, 8] with differences (16, -7, 3, -3, 6, -8).
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> nums = [1,2,3,4,5,6,7,8,9]
<strong>Output:</strong> 2
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>0 &lt;= nums[i] &lt;= 1000</code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Follow up:</strong> Could you solve this in <code>O(n)</code> time?</p>


**Companies**:  
[Apple](https://leetcode.com/company/apple), [Amazon](https://leetcode.com/company/amazon)

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/), [Greedy](https://leetcode.com/tag/greedy/)

## Solution 1. Recursion + Memoization

```cpp
// OJ: https://leetcode.com/problems/wiggle-subsequence/
// Author: A M A N
// Time : O(N^2)
// Space: O(N^2)
class Solution {
public:
    int dp[1001][1001][2];
    int solve(vector<int>&nums, int idx, int prev, bool increasing){
        if(idx>=nums.size())    return 0;
        if(dp[idx][prev][increasing]!=-1)
            return dp[idx][prev][increasing];
        int ans = 0;
        if(increasing and nums[idx]>prev){ // pick if the current element follows the order
            if(idx==0)  ans = max(1 + solve(nums, idx+1, nums[idx], false), 1+ solve(nums, idx+1, nums[idx], true));
            else        ans = 1 + solve(nums, idx+1, nums[idx], false); 
        }
        else if(!increasing and nums[idx]<prev)
            ans = 1 + solve(nums, idx+1, nums[idx], true);
        ans =  max(ans, solve(nums, idx+1, prev, increasing)); // max(pick, skip)
        return dp[idx][prev][increasing] = ans;
    }
    int wiggleMaxLength(vector<int>& nums) {
        memset(dp, -1, sizeof dp);
        return max(1, solve(nums, 0, 0, 1));
    }
};
```

## Solution 2. Greedy

```cpp
// OJ: https://leetcode.com/problems/wiggle-subsequence/solution/
// Author: A M A N
// Time : O(N)
// Space: O(1)
class Solution {
public:
    int wiggleMaxLength(vector<int>& nums) {
        if(nums.size()<2) return nums.size();
        int ans = 1, sign = 0;
        for(int i=1; i<nums.size(); i++){
            if(nums[i] > nums[i-1] and sign!=-1){
                sign = -1;
                ans++; // no of peaks
            }else if(nums[i] < nums[i-1] and sign!=1){
                sign = 1;
                ans++; // no of valleys
            }
        }
        return ans;
    }
};
```