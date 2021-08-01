# [34. Find First and Last Position of Element in Sorted Array (Medium)](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

<p>Given an array of integers <code>nums</code> sorted in ascending order, find the starting and ending position of a given <code>target</code> value.</p>

<p>If <code>target</code> is not found in the array, return <code>[-1, -1]</code>.</p>

<p>You must&nbsp;write an algorithm with&nbsp;<code>O(log n)</code> runtime complexity.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<pre><strong>Input:</strong> nums = [5,7,7,8,8,10], target = 8
<strong>Output:</strong> [3,4]
</pre><p><strong>Example 2:</strong></p>
<pre><strong>Input:</strong> nums = [5,7,7,8,8,10], target = 6
<strong>Output:</strong> [-1,-1]
</pre><p><strong>Example 3:</strong></p>
<pre><strong>Input:</strong> nums = [], target = 0
<strong>Output:</strong> [-1,-1]
</pre>
<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>0 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>9</sup>&nbsp;&lt;= nums[i]&nbsp;&lt;= 10<sup>9</sup></code></li>
	<li><code>nums</code> is a non-decreasing array.</li>
	<li><code>-10<sup>9</sup>&nbsp;&lt;= target&nbsp;&lt;= 10<sup>9</sup></code></li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Binary Search](https://leetcode.com/tag/binary-search/)

**Similar Questions**:
* [First Bad Version (Easy)](https://leetcode.com/problems/first-bad-version/)

## Solution 1. Binary Search using STL

```cpp
// OJ: https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/
// Author: A M A N
// Time : O(logN)
// Space: O(1)
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        if(!binary_search(nums.begin(), nums.end(), target))
            return {-1,-1};
        int first  = lower_bound(nums.begin(), nums.end(), target) - nums.begin();
        int second = upper_bound(nums.begin(), nums.end(), target) - nums.begin() - 1; // upper bound returns the next pointer to the found target so -1 
        return {first, second};
    }
};
```


## Solution 2. Binary Search

```cpp
// OJ: https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/
// Author: A M A N
// Time : O(logN)
// Space: O(1)
class Solution {
public:
    int findFirst(vector<int>&nums, int target){
        int res = -1;
        int L = 0, R = nums.size()-1;
        while(L<=R){
            int M = L + (R-L)/2;
            if(nums[M]==target){
                res = M; // storing candidates
                R = M-1; // moving left
            } else if(nums[M]>target)
                R = M-1;
            else 
                L = M+1;
        }
        return res;
    }
    int findLast(vector<int>&nums, int target){
        int res = -1;
        int L = 0, R = nums.size()-1;
        while(L<=R){
            int M = L + (R-L)/2;
            if(nums[M]==target){
                res = M; // storing candidates
                L = M+1; // moving right
            } else if(nums[M]>target)
                R = M-1;
            else 
                L = M+1;
        }
        return res;
    }    
    vector<int> searchRange(vector<int>& nums, int target) {
        int first  = findFirst(nums, target);
        int second = findLast(nums, target);
        return {first, second};
    }
};
```