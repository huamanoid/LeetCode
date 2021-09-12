# [718. Maximum Length of Repeated Subarray (Medium)](https://leetcode.com/problems/maximum-length-of-repeated-subarray/)

<p>Given two integer arrays <code>nums1</code> and <code>nums2</code>, return <em>the maximum length of a subarray that appears in <strong>both</strong> arrays</em>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> nums1 = [1,2,3,2,1], nums2 = [3,2,1,4,7]
<strong>Output:</strong> 3
<strong>Explanation:</strong> The repeated subarray with maximum length is [3,2,1].
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> nums1 = [0,0,0,0,0], nums2 = [0,0,0,0,0]
<strong>Output:</strong> 5
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums1.length, nums2.length &lt;= 1000</code></li>
	<li><code>0 &lt;= nums1[i], nums2[i] &lt;= 100</code></li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Binary Search](https://leetcode.com/tag/binary-search/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/), [Sliding Window](https://leetcode.com/tag/sliding-window/), [Rolling Hash](https://leetcode.com/tag/rolling-hash/), [Hash Function](https://leetcode.com/tag/hash-function/)

**Similar Questions**:
* [Minimum Size Subarray Sum (Medium)](https://leetcode.com/problems/minimum-size-subarray-sum/)
* [Longest Common Subpath (Hard)](https://leetcode.com/problems/longest-common-subpath/)

## Solution 1. DP 

```cpp
// OJ: https://leetcode.com/problems/maximum-length-of-repeated-subarray/
// Author: A M A N
// Time : O(N * M)
// Space: O(N * M)
class Solution {
public:
    int findLength(vector<int>& nums1, vector<int>& nums2) {
        int n = nums1.size(), m = nums2.size();
        int dp[n+1][m+1];
        for(int i=0; i<=n; i++) dp[i][0] = 0;
        for(int j=0; j<=m; j++) dp[0][j] = 0;
        for(int i=1; i<=n; i++)
            for(int j=1; j<=m; j++)
                if(nums1[i-1]==nums2[j-1])
                    dp[i][j] =  1 + dp[i-1][j-1];
                else
                    dp[i][j] = 0;
        int mx = 0;
        for(int i=0; i<=n; i++)
            for(int j=0; j<=m; j++)
                mx = max(mx, dp[i][j]);
        return mx;
    }
};
```

## Solution 2. Recursion + Memoization

```cpp
// OJ: https://leetcode.com/problems/maximum-length-of-repeated-subarray/
// Author: A M A N
// Time : O(N * M)
// Space: O(N * M)
class Solution {
    int n, m;
    vector<int>A, B;
    int dp[1001][1001][2];
public:
    int solve(int i, int j, bool selected){
        if(i>=n or j>=m) return 0;
        if(dp[i][j][selected]!=-1) return dp[i][j][selected];
        if(selected){
            if(A[i]!=B[j])
                return dp[i][j][selected] = 0;
            else
                return dp[i][j][selected] = 1 + solve(i+1, j+1, true);
        }
        int a = INT_MIN;
        if(A[i]==B[j]){
            a = 1 + solve(i+1, j+1, true);
        }
        int b = solve(i+1, j, false);
        int c = solve(i, j+1, false);
        return dp[i][j][selected] = max({a, b, c});
    }
    int findLength(vector<int>& nums1, vector<int>& nums2) {
        A = nums1, B = nums2;
        n = nums1.size(), m = nums2.size();
        memset(dp, -1, sizeof dp);
        return solve(0, 0, false);
    }
};       
```