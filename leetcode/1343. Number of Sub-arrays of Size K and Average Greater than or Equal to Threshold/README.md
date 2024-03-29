# [1343. Number of Sub-arrays of Size K and Average Greater than or Equal to Threshold (Medium)](https://leetcode.com/problems/number-of-sub-arrays-of-size-k-and-average-greater-than-or-equal-to-threshold/)

<p>Given an array of integers <code>arr</code> and two integers <code>k</code> and <code>threshold</code>.</p>

<p>Return <em>the number of sub-arrays</em> of size <code>k</code> and average greater than or equal to <code>threshold</code>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> arr = [2,2,2,2,5,5,5,8], k = 3, threshold = 4
<strong>Output:</strong> 3
<strong>Explanation:</strong> Sub-arrays [2,5,5],[5,5,5] and [5,5,8] have averages 4, 5 and 6 respectively. All other sub-arrays of size 3 have averages less than 4 (the threshold).
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> arr = [1,1,1,1,1], k = 1, threshold = 0
<strong>Output:</strong> 5
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> arr = [11,13,17,23,29,31,7,5,2,3], k = 3, threshold = 5
<strong>Output:</strong> 6
<strong>Explanation:</strong> The first 6 sub-arrays of size 3 have averages greater than 5. Note that averages are not integers.
</pre>

<p><strong>Example 4:</strong></p>

<pre><strong>Input:</strong> arr = [7,7,7,7,7,7,7], k = 7, threshold = 7
<strong>Output:</strong> 1
</pre>

<p><strong>Example 5:</strong></p>

<pre><strong>Input:</strong> arr = [4,4,4,4], k = 4, threshold = 1
<strong>Output:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= arr.length &lt;= 10^5</code></li>
	<li><code>1 &lt;= arr[i] &lt;= 10^4</code></li>
	<li><code>1 &lt;= k &lt;= arr.length</code></li>
	<li><code>0 &lt;= threshold &lt;= 10^4</code></li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Sliding Window](https://leetcode.com/tag/sliding-window/)

## Solution 1. Sliding Window

```cpp
// OJ: https://leetcode.com/problems/number-of-sub-arrays-of-size-k-and-average-greater-than-or-equal-to-threshold/
// Author: A M A N
// Time : O(N)
// Space: O(1)
class Solution {
public:
    int numOfSubarrays(vector<int>& arr, int k, int threshold) {
        int ans = 0, sum=0;
        int i=0, j = 0;
        while(j<arr.size()){
            sum+=arr[j];
            int window = j-i+1;
            if(window<k)
                j++;
            else if(window==k){
                if(sum/k>=threshold)ans++;
                sum-=arr[i];
                i++, j++;
            }
        }
        return ans;
    }
};
```