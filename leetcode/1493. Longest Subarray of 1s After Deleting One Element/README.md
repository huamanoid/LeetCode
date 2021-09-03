# [1493. Longest Subarray of 1's After Deleting One Element (Medium)](https://leetcode.com/problems/longest-subarray-of-1s-after-deleting-one-element/)

<p>Given a binary array <code>nums</code>, you should delete one element from it.</p>

<p>Return the size of the longest non-empty subarray containing only 1's&nbsp;in the resulting array.</p>

<p>Return 0 if there is no such subarray.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> nums = [1,1,0,1]
<strong>Output:</strong> 3
<strong>Explanation: </strong>After deleting the number in position 2, [1,1,1] contains 3 numbers with value of 1's.</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> nums = [0,1,1,1,0,1,1,0,1]
<strong>Output:</strong> 5
<strong>Explanation: </strong>After deleting the number in position 4, [0,1,1,1,1,1,0,1] longest subarray with value of 1's is [1,1,1,1,1].</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> nums = [1,1,1]
<strong>Output:</strong> 2
<strong>Explanation: </strong>You must delete one element.</pre>

<p><strong>Example 4:</strong></p>

<pre><strong>Input:</strong> nums = [1,1,0,0,1,1,1,0,1]
<strong>Output:</strong> 4
</pre>

<p><strong>Example 5:</strong></p>

<pre><strong>Input:</strong> nums = [0,0,0]
<strong>Output:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10^5</code></li>
	<li><code>nums[i]</code>&nbsp;is either&nbsp;<code>0</code>&nbsp;or&nbsp;<code>1</code>.</li>
</ul>


**Related Topics**:  
[Math](https://leetcode.com/tag/math/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/), [Sliding Window](https://leetcode.com/tag/sliding-window/)

## Solution 1. Sliding Window

```cpp
// OJ: https://leetcode.com/problems/longest-subarray-of-1s-after-deleting-one-element/
// Author: A M A N
// Time : O(N)
// Space: O(1)
class Solution {
public:
    int longestSubarray(vector<int>& nums) {
        // solving for flipping k zeros instead of deleting and subtracting k at the end
        int ans = 0;
        int i=0, j=0, k=1;
        while(j<nums.size()){
            if(nums[j]==0)k--;
            while(k<0){
                if(nums[i]==0)k++;
                i++;
            }
            ans = max(ans, j-i+1);
            j++;
        }
        return ans-1;
    }
};
```