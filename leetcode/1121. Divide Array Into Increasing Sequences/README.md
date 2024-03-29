# [1121. Divide Array Into Increasing Sequences (Hard)](https://leetcode.com/problems/divide-array-into-increasing-sequences/)

<p>Given an integer array <code>nums</code> sorted in non-decreasing order and an integer <code>k</code>, return <code>true</code><em> if this array can be divided into one or more disjoint increasing subsequences of length at least </em><code>k</code><em>, or </em><code>false</code><em> otherwise</em>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> nums = [1,2,2,3,3,4,4], k = 3
<strong>Output:</strong> true
<strong>Explanation:</strong> The array can be divided into two subsequences [1,2,3,4] and [2,3,4] with lengths at least 3 each.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> nums = [5,6,6,7,8], k = 3
<strong>Output:</strong> false
<strong>Explanation:</strong> There is no way to divide the array using the conditions required.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>nums</code> is sorted in non-decreasing order.</li>
</ul>


**Companies**:  
[Google](https://leetcode.com/company/google)

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Greedy](https://leetcode.com/tag/greedy/)

## Solution 1.

```cpp
// OJ: https://leetcode.com/problems/divide-array-into-increasing-sequences/
// Author: A M A N
// Time : O(N)
// Space: O(1)
class Solution {
public:
    bool canDivideIntoSubsequences(vector<int>& nums, int k) {
        int same = 0, cnt=1;
        for(int i=1; i<nums.size(); i++){
            if(nums[i]==nums[i-1]){
                cnt++;
            }else
                cnt=1;
            same = max(same, cnt);
        }
        return same*k<=nums.size();
    }
};
```