# [162. Find Peak Element (Medium)](https://leetcode.com/problems/find-peak-element/)

<p>A peak element is an element that is strictly greater than its neighbors.</p>

<p>Given an integer array <code>nums</code>, find a peak element, and return its index. If&nbsp;the array contains multiple peaks, return the index to <strong>any of the peaks</strong>.</p>

<p>You may imagine that <code>nums[-1] = nums[n] = -∞</code>.</p>

<p>You must write an algorithm that runs in&nbsp;<code>O(log n)</code> time.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> nums = [1,2,3,1]
<strong>Output:</strong> 2
<strong>Explanation:</strong> 3 is a peak element and your function should return the index number 2.</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> nums = [1,2,1,3,5,6,4]
<strong>Output:</strong> 5
<strong>Explanation:</strong> Your function can return either index number 1 where the peak element is 2, or index number 5 where the peak element is 6.</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>-2<sup>31</sup> &lt;= nums[i] &lt;= 2<sup>31</sup> - 1</code></li>
	<li><code>nums[i] != nums[i + 1]</code> for all valid <code>i</code>.</li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Binary Search](https://leetcode.com/tag/binary-search/)

**Similar Questions**:
* [Peak Index in a Mountain Array (Easy)](https://leetcode.com/problems/peak-index-in-a-mountain-array/)
* [Find a Peak Element II (Medium)](https://leetcode.com/problems/find-a-peak-element-ii/)


## Solution 1. Binary Search (L<=R)

```cpp
// OJ: https://leetcode.com/problems/find-peak-element/
// Author: A M A N
// Time : O(logN)
// Space: O(1)
class Solution {
public:
    int findPeakElement(vector<int>& nums) {
        int L = 0, R = nums.size()-1;
        while(L<=R){
            long M = (L+R)/2;
            long prev = M==0 ? LONG_MIN : nums[M-1], next = M==nums.size()-1 ? LONG_MIN : nums[M+1];
            if(nums[M]>prev and nums[M]>next)
                return M;
            else if(nums[M] < prev) //since left side has already one candidate (prev) which is bigger than ONE of its neighbour so it's better to move left.
                R = M-1;
            else
                L = M+1;
        }
        return -1;
    }
};
```


## Solution 2. Binary Search (L<R)

```cpp
// OJ: https://leetcode.com/problems/find-peak-element/
// Author: A M A N
// Time : O(logN)
// Space: O(1)
class Solution {
public:
    int findPeakElement(vector<int>& nums) {
        int L = 0, R = nums.size()-1;
        while(L<R){
            int M = (L+R)/2;
            if(nums[M]>nums[M+1])
                R = M;
            else
                L = M+1;
        }
        return L;
    }
};
```