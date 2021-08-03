# [4. Median of Two Sorted Arrays (Hard)](https://leetcode.com/problems/median-of-two-sorted-arrays/)

<p>Given two sorted arrays <code>nums1</code> and <code>nums2</code> of size <code>m</code> and <code>n</code> respectively, return <strong>the median</strong> of the two sorted arrays.</p>

<p>The overall run time complexity should be <code>O(log (m+n))</code>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> nums1 = [1,3], nums2 = [2]
<strong>Output:</strong> 2.00000
<strong>Explanation:</strong> merged array = [1,2,3] and median is 2.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> nums1 = [1,2], nums2 = [3,4]
<strong>Output:</strong> 2.50000
<strong>Explanation:</strong> merged array = [1,2,3,4] and median is (2 + 3) / 2 = 2.5.
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> nums1 = [0,0], nums2 = [0,0]
<strong>Output:</strong> 0.00000
</pre>

<p><strong>Example 4:</strong></p>

<pre><strong>Input:</strong> nums1 = [], nums2 = [1]
<strong>Output:</strong> 1.00000
</pre>

<p><strong>Example 5:</strong></p>

<pre><strong>Input:</strong> nums1 = [2], nums2 = []
<strong>Output:</strong> 2.00000
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>nums1.length == m</code></li>
	<li><code>nums2.length == n</code></li>
	<li><code>0 &lt;= m &lt;= 1000</code></li>
	<li><code>0 &lt;= n &lt;= 1000</code></li>
	<li><code>1 &lt;= m + n &lt;= 2000</code></li>
	<li><code>-10<sup>6</sup> &lt;= nums1[i], nums2[i] &lt;= 10<sup>6</sup></code></li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Binary Search](https://leetcode.com/tag/binary-search/), [Divide and Conquer](https://leetcode.com/tag/divide-and-conquer/)

## Solution 1.

```cpp
// OJ: https://leetcode.com/problems/median-of-two-sorted-arrays/
// Author: A M A N
// Time : O(log(min(N, M)))
// Space: O(1)
// Ref  : https://www.youtube.com/watch?v=yD7wV8SyPrc
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int n = nums1.size(), m = nums2.size();
        if(n>m)swap(nums1, nums2), swap(n, m); // because there is possibility that cut2 will go out of bounds if we take larger array for binary search.
        int L = 0, R = n; // here R is n instead of n-1 because, Here L and R is taken for partition, for example arr = [1 2 3 4 5 6], one partition may arise where, [1 2 3 4 5 6] | [empty]
        while(L<=R){
            int cut1 = (L+R)/2; // no of elements before the cut in first array
            int cut2 = (n+m)/2 - cut1; // no of elements before the cut in second array. 
            //since total number of elements before the cut HAVE to be half of all the elements
            int l1 = cut1==0 ? INT_MIN : nums1[cut1-1]; // no elements before cut1 so -infinity
            int l2 = cut2==0 ? INT_MIN : nums2[cut2-1]; // no elements before cut2 so -infinity
            int r1 = cut1==n ? INT_MAX : nums1[cut1];   // cut1 exists at the end i.e. all elements are before the cut1 
            int r2 = cut2==m ? INT_MAX : nums2[cut2];   // cut2 exisits at the end i.e. all elements are before the cut2
            // The proper cut1 and cut2 must satisfy the sorted property viz.
            // ALL THE RIGHT ELEMENTS MUST BE GREATER OR EQUAL TO LEFT ELEMENTS.
            // l1 < r1 && l2 < r2  (since the arrays are sorted this property is already satisfied)
            // l1 < r2 && l2 < r1  (we've to check this only)
// Since cut1 and cut2 are dependent we will only work out for first array and find the proper cut1
            if(l1>r2) // l1 is bigger than r2, we've to decrease l1 so reduce the (Right) upper boundary
                R = cut1 - 1; 
            else if(l2>r1)// r1 is smaller than l2, we've to increase r1, increase the (left)lower bound
                L = cut1 + 1;
            else
                if((n+m)%2==0) // even 
                    return (max(l1, l2) + min(r1, r2))/2.0;
                else // odd
                    return min(r1, r2);
        }
        return -3;
    }
};
```