# [801. Minimum Swaps To Make Sequences Increasing (Hard)](https://leetcode.com/problems/minimum-swaps-to-make-sequences-increasing/)

<p>You are given two integer arrays of the same length <code>nums1</code> and <code>nums2</code>. In one operation, you are allowed to swap <code>nums1[i]</code> with <code>nums2[i]</code>.</p>

<ul>
	<li>For example, if <code>nums1 = [1,2,3,<u>8</u>]</code>, and <code>nums2 = [5,6,7,<u>4</u>]</code>, you can swap the element at <code>i = 3</code> to obtain <code>nums1 = [1,2,3,4]</code> and <code>nums2 = [5,6,7,8]</code>.</li>
</ul>

<p>Return <em>the minimum number of needed operations to make </em><code>nums1</code><em> and </em><code>nums2</code><em> <strong>strictly increasing</strong></em>. The test cases are generated so that the given input always makes it possible.</p>

<p>An array <code>arr</code> is <strong>strictly increasing</strong> if and only if <code>arr[0] &lt; arr[1] &lt; arr[2] &lt; ... &lt; arr[arr.length - 1]</code>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> nums1 = [1,3,5,4], nums2 = [1,2,3,7]
<strong>Output:</strong> 1
<strong>Explanation:</strong> 
Swap nums1[3] and nums2[3]. Then the sequences are:
nums1 = [1, 3, 5, 7] and nums2 = [1, 2, 3, 4]
which are both strictly increasing.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> nums1 = [0,3,5,8,9], nums2 = [2,1,4,6,9]
<strong>Output:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>2 &lt;= nums1.length &lt;= 10<sup>5</sup></code></li>
	<li><code>nums2.length == nums1.length</code></li>
	<li><code>0 &lt;= nums1[i], nums2[i] &lt;= 2 * 10<sup>5</sup></code></li>
</ul>


**Companies**:  
[Google](https://leetcode.com/company/google)

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/)

## Solution 1. Recursion + Memoization

```cpp
// OJ: https://leetcode.com/problems/minimum-swaps-to-make-sequences-increasing/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    const static int mxN = 1e5+2;
    int dp[mxN];
    int solve(vector<int>&A, vector<int>&B, int n){
        if(n>=A.size()-1) return 0;
        if(dp[n]!=-1)
            return dp[n];
        if(A[n]<A[n+1] and B[n]<B[n+1]){
            return dp[n] = solve(A, B, n+1);
        }
        // swap(A[n+1], B[n+1]);
        return dp[n] = 1+solve(A, B, n+1);
    }
    int minSwap(vector<int>& nums1, vector<int>& nums2) {
        memset(dp, -1, sizeof dp);
        return solve(nums1,nums2, 0);
    }
};
```