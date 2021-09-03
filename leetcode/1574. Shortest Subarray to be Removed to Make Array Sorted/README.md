# [1574. Shortest Subarray to be Removed to Make Array Sorted (Medium)](https://leetcode.com/problems/shortest-subarray-to-be-removed-to-make-array-sorted/)

<p>Given an integer array&nbsp;<code>arr</code>, remove a&nbsp;subarray (can be empty) from&nbsp;<code>arr</code>&nbsp;such that the remaining elements in <code>arr</code>&nbsp;are <strong>non-decreasing</strong>.</p>

<p>A subarray is a contiguous&nbsp;subsequence of the array.</p>

<p>Return&nbsp;<em>the length of the shortest subarray to remove</em>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> arr = [1,2,3,10,4,2,3,5]
<strong>Output:</strong> 3
<strong>Explanation: </strong>The shortest subarray we can remove is [10,4,2] of length 3. The remaining elements after that will be [1,2,3,3,5] which are sorted.
Another correct solution is to remove the subarray [3,10,4].</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> arr = [5,4,3,2,1]
<strong>Output:</strong> 4
<strong>Explanation: </strong>Since the array is strictly decreasing, we can only keep a single element. Therefore we need to remove a subarray of length 4, either [5,4,3,2] or [4,3,2,1].
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> arr = [1,2,3]
<strong>Output:</strong> 0
<strong>Explanation: </strong>The array is already non-decreasing. We do not need to remove any elements.
</pre>

<p><strong>Example 4:</strong></p>

<pre><strong>Input:</strong> arr = [1]
<strong>Output:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= arr.length &lt;= 10^5</code></li>
	<li><code>0 &lt;= arr[i] &lt;= 10^9</code></li>
</ul>


**Companies**:  
[Amazon](https://leetcode.com/company/amazon), [Flipkart](https://leetcode.com/company/flipkart)

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Two Pointers](https://leetcode.com/tag/two-pointers/), [Binary Search](https://leetcode.com/tag/binary-search/), [Stack](https://leetcode.com/tag/stack/), [Monotonic Stack](https://leetcode.com/tag/monotonic-stack/)

## Solution 1. Two Pointers

```cpp
// OJ: https://leetcode.com/problems/shortest-subarray-to-be-removed-to-make-array-sorted/
// Author: A M A N
// Time : O(N)
// Space: O(1)
class Solution {
public:
    int findLengthOfShortestSubarray(vector<int>& arr) {
        int N = arr.size();
        int left = 0, right = N-1;
        while(left+1<N and arr[left]<=arr[left+1])left++; // first valley
        if(left==N-1)return 0; // the array is non-decreasing itself
        while(right-1>=0 and arr[right]>=arr[right-1])right--; // last valley
        int i=0, j = right;
        int ans = min(N - left-1, right); // removing min of [0...last valley] or [first valley ... N-1]
        while (i <= left && j < N) { // finding the least size window when after removed the first and last part forms a non-decreasing sequence
            if (arr[j] >= arr[i]) {
                ans = min(ans, j - i - 1);
                ++i;
            } else ++j;
        }
        return ans;
    }
};
```