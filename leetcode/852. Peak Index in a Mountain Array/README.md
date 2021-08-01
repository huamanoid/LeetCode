# [852. Peak Index in a Mountain Array (Easy)](https://leetcode.com/problems/peak-index-in-a-mountain-array/)

<p>Let's call an array <code>arr</code> a <strong>mountain</strong>&nbsp;if the following properties hold:</p>

<ul>
	<li><code>arr.length &gt;= 3</code></li>
	<li>There exists some <code>i</code> with&nbsp;<code>0 &lt; i&nbsp;&lt; arr.length - 1</code>&nbsp;such that:
	<ul>
		<li><code>arr[0] &lt; arr[1] &lt; ... arr[i-1] &lt; arr[i] </code></li>
		<li><code>arr[i] &gt; arr[i+1] &gt; ... &gt; arr[arr.length - 1]</code></li>
	</ul>
	</li>
</ul>

<p>Given an integer array <code>arr</code> that is <strong>guaranteed</strong> to be&nbsp;a mountain, return any&nbsp;<code>i</code>&nbsp;such that&nbsp;<code>arr[0] &lt; arr[1] &lt; ... arr[i - 1] &lt; arr[i] &gt; arr[i + 1] &gt; ... &gt; arr[arr.length - 1]</code>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<pre><strong>Input:</strong> arr = [0,1,0]
<strong>Output:</strong> 1
</pre><p><strong>Example 2:</strong></p>
<pre><strong>Input:</strong> arr = [0,2,1,0]
<strong>Output:</strong> 1
</pre><p><strong>Example 3:</strong></p>
<pre><strong>Input:</strong> arr = [0,10,5,2]
<strong>Output:</strong> 1
</pre><p><strong>Example 4:</strong></p>
<pre><strong>Input:</strong> arr = [3,4,5,1]
<strong>Output:</strong> 2
</pre><p><strong>Example 5:</strong></p>
<pre><strong>Input:</strong> arr = [24,69,100,99,79,78,67,36,26,19]
<strong>Output:</strong> 2
</pre>
<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>3 &lt;= arr.length &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= arr[i] &lt;= 10<sup>6</sup></code></li>
	<li><code>arr</code> is <strong>guaranteed</strong> to be a mountain array.</li>
</ul>

<p>&nbsp;</p>
<strong>Follow up:</strong> Finding the <code>O(n)</code> is straightforward, could you find an <code>O(log(n))</code> solution?

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Binary Search](https://leetcode.com/tag/binary-search/)

**Similar Questions**:
* [Find Peak Element (Medium)](https://leetcode.com/problems/find-peak-element/)
* [Find in Mountain Array (Hard)](https://leetcode.com/problems/find-in-mountain-array/)
* [Minimum Number of Removals to Make Mountain Array (Hard)](https://leetcode.com/problems/minimum-number-of-removals-to-make-mountain-array/)

## Solution 1. Binary Search

```cpp
// OJ: https://leetcode.com/problems/peak-index-in-a-mountain-array/
// Author: A M A N
// Time : O(logN)
// Space: O(1)
class Solution {
public:
    int peakIndexInMountainArray(vector<int>& nums) {
        int n = nums.size();
        int start = 0;
        int end = n-1;
        while(start<=end){
            int mid = start + (end-start)/2;
            if(mid>0 and mid<n-1){
                if(nums[mid]>nums[mid-1] and nums[mid]> nums[mid+1]) //peak element property
                    return mid;
                else if(nums[mid]>nums[mid-1])
                    start = mid+1;
                else
                    end = mid-1;
            }else if(mid==0){ // can't be a peak at the beginning
                start = mid+1; // peak must exist on the right
            }else // can't be a peak at the end
                end = mid-1; // peak must exist on the left 
        }
        return 0;
    }
};
```