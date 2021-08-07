# [992. Subarrays with K Different Integers (Hard)](https://leetcode.com/problems/subarrays-with-k-different-integers/)

<p>Given an integer array <code>nums</code> and an integer <code>k</code>, return <em>the number of <strong>good subarrays</strong> of </em><code>nums</code>.</p>

<p>A <strong>good array</strong> is an array where the number of different integers in that array is exactly <code>k</code>.</p>

<ul>
	<li>For example, <code>[1,2,3,1,2]</code> has <code>3</code> different integers: <code>1</code>, <code>2</code>, and <code>3</code>.</li>
</ul>

<p>A <strong>subarray</strong> is a <strong>contiguous</strong> part of an array.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> nums = [1,2,1,2,3], k = 2
<strong>Output:</strong> 7
<strong>Explanation:</strong> Subarrays formed with exactly 2 different integers: [1,2], [2,1], [1,2], [2,3], [1,2,1], [2,1,2], [1,2,1,2]
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> nums = [1,2,1,3,4], k = 3
<strong>Output:</strong> 3
<strong>Explanation:</strong> Subarrays formed with exactly 3 different integers: [1,2,1,3], [2,1,3], [1,3,4].
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= nums[i], k &lt;= nums.length</code></li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Hash Table](https://leetcode.com/tag/hash-table/), [Sliding Window](https://leetcode.com/tag/sliding-window/), [Counting](https://leetcode.com/tag/counting/)

**Similar Questions**:
* [Longest Substring Without Repeating Characters (Medium)](https://leetcode.com/problems/longest-substring-without-repeating-characters/)
* [Longest Substring with At Most Two Distinct Characters (Medium)](https://leetcode.com/problems/longest-substring-with-at-most-two-distinct-characters/)
* [Longest Substring with At Most K Distinct Characters (Medium)](https://leetcode.com/problems/longest-substring-with-at-most-k-distinct-characters/)

## Solution 1. Sliding Window

```cpp
// OJ: https://leetcode.com/problems/subarrays-with-k-different-integers/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    int subarraysWithKDistinct(vector<int>& nums, int k) {
        int ans = 0; 
        int i=0, j=0;
        unordered_map<int, int> mp;
        while(j<nums.size()){
            mp[nums[j]]++;
            if(mp.size()<k)
                j++;
            else if(mp.size()==k){
                while(mp.size()==k){
                    int x = j+1; 
                    while(x<nums.size() and mp.find(nums[x])!=mp.end())x++; 
                    // x is the index of next number which is not present in sub-array/map, 
                    // so all the subarrays nums[i, j to x-1] will have K different integers only
                    ans += x-j;
                    mp[nums[i]]--;
                    if(mp[nums[i]]==0)mp.erase(nums[i]);
                    i++; //shrink the window
                }
                j++;// extend the window
            } 
        }
        return ans;
    }
};
```