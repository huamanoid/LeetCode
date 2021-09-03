# [1248. Count Number of Nice Subarrays (Medium)](https://leetcode.com/problems/count-number-of-nice-subarrays/)

<p>Given an array of integers <code>nums</code> and an integer <code>k</code>. A continuous subarray is called <strong>nice</strong> if there are <code>k</code> odd numbers on it.</p>

<p>Return <em>the number of <strong>nice</strong> sub-arrays</em>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> nums = [1,1,2,1,1], k = 3
<strong>Output:</strong> 2
<strong>Explanation:</strong> The only sub-arrays with 3 odd numbers are [1,1,2,1] and [1,2,1,1].
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> nums = [2,4,6], k = 1
<strong>Output:</strong> 0
<strong>Explanation:</strong> There is no odd numbers in the array.
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> nums = [2,2,2,1,2,2,1,2,2,2], k = 2
<strong>Output:</strong> 16
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 50000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10^5</code></li>
	<li><code>1 &lt;= k &lt;= nums.length</code></li>
</ul>

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Hash Table](https://leetcode.com/tag/hash-table/), [Math](https://leetcode.com/tag/math/), [Sliding Window](https://leetcode.com/tag/sliding-window/)

## Solution 1. Sliding window

```
 k = 2
 nums = [2,2,2,1,2,2,1,2,2,2,1,2]
  (k*)  [0,0,0,1,1,1,2,2,2,2,3,3]
        [            ] ] ] ] X
           [         ] ] ] ] X
             [       ] ] ] ] X
               [     ] ] ] ] X
                 .
                 .
                 .
                 .

        once nums[i, j] contains k odd numbers. subarrays nums[i, j to X] all will have k odd numbers.
        where X is the index of next odd element. 
```

```cpp
// OJ: https://leetcode.com/problems/count-number-of-nice-subarrays/
// Author: A M A N
// Time : O(N)
// Space: O(1)
class Solution {
public:
    int numberOfSubarrays(vector<int>& nums, int k) {
        int ans = 0,  cnt=0;
        int i=0, j=0;
        while(j<nums.size()){
            if(nums[j]&1)cnt++;
            if(cnt<k)
                j++;
            else if(cnt==k){
                int x = j+1;
                while(x<nums.size() and nums[x]&1^1)x++;
                while(cnt==k){
                    ans += x - j; // x is the position of next odd element, before that all the sub arrays will be have cnt==k only.
                    if(nums[i]&1)cnt--;
                    i++;
                }
                j++;
            }
        }
        return ans;
    }
};
```