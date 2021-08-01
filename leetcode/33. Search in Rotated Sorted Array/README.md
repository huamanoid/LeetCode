# [33. Search in Rotated Sorted Array (Medium)](https://leetcode.com/problems/search-in-rotated-sorted-array/)

<p>There is an integer array <code>nums</code> sorted in ascending order (with <strong>distinct</strong> values).</p>

<p>Prior to being passed to your function, <code>nums</code> is <strong>rotated</strong> at an unknown pivot index <code>k</code> (<code>0 &lt;= k &lt; nums.length</code>) such that the resulting array is <code>[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]</code> (<strong>0-indexed</strong>). For example, <code>[0,1,2,4,5,6,7]</code> might be rotated at pivot index <code>3</code> and become <code>[4,5,6,7,0,1,2]</code>.</p>

<p>Given the array <code>nums</code> <strong>after</strong> the rotation and an integer <code>target</code>, return <em>the index of </em><code>target</code><em> if it is in </em><code>nums</code><em>, or </em><code>-1</code><em> if it is not in </em><code>nums</code>.</p>

<p>You must&nbsp;write an algorithm with&nbsp;<code>O(log n)</code> runtime complexity.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<pre><strong>Input:</strong> nums = [4,5,6,7,0,1,2], target = 0
<strong>Output:</strong> 4
</pre><p><strong>Example 2:</strong></p>
<pre><strong>Input:</strong> nums = [4,5,6,7,0,1,2], target = 3
<strong>Output:</strong> -1
</pre><p><strong>Example 3:</strong></p>
<pre><strong>Input:</strong> nums = [1], target = 0
<strong>Output:</strong> -1
</pre>
<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 5000</code></li>
	<li><code>-10<sup>4</sup> &lt;= nums[i] &lt;= 10<sup>4</sup></code></li>
	<li>All values of <code>nums</code> are <strong>unique</strong>.</li>
	<li><code>nums</code> is guaranteed to be rotated at some pivot.</li>
	<li><code>-10<sup>4</sup> &lt;= target &lt;= 10<sup>4</sup></code></li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Binary Search](https://leetcode.com/tag/binary-search/)

**Similar Questions**:
* [Search in Rotated Sorted Array II (Medium)](https://leetcode.com/problems/search-in-rotated-sorted-array-ii/)
* [Find Minimum in Rotated Sorted Array (Medium)](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)

## Solution 1.

```cpp
// OJ: https://leetcode.com/problems/search-in-rotated-sorted-array/
// Author: A M A N
// Time : O(logN)
// Space: O(1)
class Solution {
public:
    int minElement(vector<int>&nums){
        int n = nums.size();
        int start = 0;
        int end = n-1;
        while(start<=end){
            int mid = start + (end-start)/2;
            int prev = (mid-1+n)%n;
            int next = (mid+1)%n;
            if(nums[prev]>= nums[mid] and nums[mid] <= nums[next]){ //min element
                return mid;
            }else if(nums[mid] >= nums[0]){// right part is unsorted, hence contains the min element
                start = mid +1;
            }else if(nums[mid] <= nums[n-1]) //left part is unsorted, hence contains the min element
                end = mid-1;
        }
        return 0;
    }
    int search(vector<int>& nums, int target) {
        int pos = minElement(nums); //index of pivot element;
        if(binary_search(nums.begin(), nums.begin()+pos, target))
            return lower_bound(nums.begin(), nums.begin()+pos, target) - nums.begin();
        else if(binary_search(nums.begin()+pos, nums.end(), target))
            return lower_bound(nums.begin()+pos, nums.end(), target) - nums.begin();
        else
            return -1;
    }
};
```
## Solution 2.

```cpp
// OJ: https://leetcode.com/problems/search-in-rotated-sorted-array/
// Author: A M A N
// Time : O(logN)
// Space: O(1)
class Solution {
public:
    int minElement(vector<int>&nums){
        int start = 0, end = nums.size()-1;
        int L = 0, R = nums.size() - 1;
        while (L < R) {
            int M = L + (R - L) / 2;
            if (nums[M] < nums[R]) R = M;
            else L = M + 1;
        }
        return L;
    }
    int search(vector<int>& nums, int target) {
        int pos = minElement(nums); //index of pivot element;
        if(binary_search(nums.begin(), nums.begin()+pos, target))
            return lower_bound(nums.begin(), nums.begin()+pos, target) - nums.begin();
        else if(binary_search(nums.begin()+pos, nums.end(), target))
            return lower_bound(nums.begin()+pos, nums.end(), target) - nums.begin();
        else
            return -1;
    }
};
```