# [153. Find Minimum in Rotated Sorted Array (Medium)](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)

<p>Suppose an array of length <code>n</code> sorted in ascending order is <strong>rotated</strong> between <code>1</code> and <code>n</code> times. For example, the array <code>nums = [0,1,2,4,5,6,7]</code> might become:</p>

<ul>
	<li><code>[4,5,6,7,0,1,2]</code> if it was rotated <code>4</code> times.</li>
	<li><code>[0,1,2,4,5,6,7]</code> if it was rotated <code>7</code> times.</li>
</ul>

<p>Notice that <strong>rotating</strong> an array <code>[a[0], a[1], a[2], ..., a[n-1]]</code> 1 time results in the array <code>[a[n-1], a[0], a[1], a[2], ..., a[n-2]]</code>.</p>

<p>Given the sorted rotated array <code>nums</code> of <strong>unique</strong> elements, return <em>the minimum element of this array</em>.</p>

<p>You must write an algorithm that runs in&nbsp;<code>O(log n) time.</code></p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> nums = [3,4,5,1,2]
<strong>Output:</strong> 1
<strong>Explanation:</strong> The original array was [1,2,3,4,5] rotated 3 times.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> nums = [4,5,6,7,0,1,2]
<strong>Output:</strong> 0
<strong>Explanation:</strong> The original array was [0,1,2,4,5,6,7] and it was rotated 4 times.
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> nums = [11,13,15,17]
<strong>Output:</strong> 11
<strong>Explanation:</strong> The original array was [11,13,15,17] and it was rotated 4 times. 
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>n == nums.length</code></li>
	<li><code>1 &lt;= n &lt;= 5000</code></li>
	<li><code>-5000 &lt;= nums[i] &lt;= 5000</code></li>
	<li>All the integers of <code>nums</code> are <strong>unique</strong>.</li>
	<li><code>nums</code> is sorted and rotated between <code>1</code> and <code>n</code> times.</li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Binary Search](https://leetcode.com/tag/binary-search/)

**Similar Questions**:
* [Search in Rotated Sorted Array (Medium)](https://leetcode.com/problems/search-in-rotated-sorted-array/)
* [Find Minimum in Rotated Sorted Array II (Hard)](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array-ii/)

## Solution 1.

```
1,2,3,4,5,6,7,8,9 =(Rotate)=> 6,7,8,9,1,2,3,4,5
```
It's not just two sorted arrays stitched at a point.
```
      9
    8
  7
6               
                5  
              4
            3
          2
        1
```
find if the mid is the min element if not move to the unsorted part (left or right)

```cpp
// OJ: https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/
// Author: A M A N
// Time : O(logN)
// Space: O(1)
class Solution {
public:
    int findMin(vector<int>& nums) {
        int n = nums.size();
        int start = 0;
        int end = n-1;
        while(start<=end){
            int mid = start + (end-start)/2;
            int prev = (mid-1+n)%n;
            int next = (mid+1)%n;
            if(nums[prev]>= nums[mid] and nums[mid] <= nums[next]){ //min element
                return nums[mid];
            }else if(nums[mid] >= nums[0]){ // [0, mid] => sorted, [mid+1, n-1] => unsorted  
                start = mid +1;
            }else if(nums[mid] <= nums[n-1])// [0, mid-1] => unsorted, [mid, n-1] => sorted  
                end = mid-1;
        }
        return nums[0];
    }
};
```

## Solution 2.

```cpp
// OJ: https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/
// Author: A M A N
// Time : O(logN)
// Space: O(1)
class Solution {
public:
    int findMin(vector<int>& nums) {
        int L = 0, R = nums.size() - 1;
        while (L < R) {
            int M = L + (R - L) / 2;
            if (nums[M] < nums[R]) R = M;
            else L = M + 1;
        }
        return nums[L];    
    }
};
```
