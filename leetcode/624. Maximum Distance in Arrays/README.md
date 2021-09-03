# [624. Maximum Distance in Arrays (Medium)](https://leetcode.com/problems/maximum-distance-in-arrays/)

<p>You are given <code>m</code> <code>arrays</code>, where each array is sorted in <strong>ascending order</strong>.</p>

<p>You can pick up two integers from two different arrays (each array picks one) and calculate the distance. We define the distance between two integers <code>a</code> and <code>b</code> to be their absolute difference <code>|a - b|</code>.</p>

<p>Return <em>the maximum distance</em>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> arrays = [[1,2,3],[4,5],[1,2,3]]
<strong>Output:</strong> 4
<strong>Explanation:</strong> One way to reach the maximum distance 4 is to pick 1 in the first or third array and pick 5 in the second array.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> arrays = [[1],[1]]
<strong>Output:</strong> 0
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> arrays = [[1],[2]]
<strong>Output:</strong> 1
</pre>

<p><strong>Example 4:</strong></p>

<pre><strong>Input:</strong> arrays = [[1,4],[0,5]]
<strong>Output:</strong> 4
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>m == arrays.length</code></li>
	<li><code>2&nbsp;&lt;= m &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= arrays[i].length &lt;= 500</code></li>
	<li><code>-10<sup>4</sup> &lt;= arrays[i][j] &lt;= 10<sup>4</sup></code></li>
	<li><code>arrays[i]</code> is sorted in <strong>ascending order</strong>.</li>
	<li>There will be at most <code>10<sup>5</sup></code> integers in all the arrays.</li>
</ul>


**Companies**:  
[Yahoo](https://leetcode.com/company/yahoo)

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Greedy](https://leetcode.com/tag/greedy/)

## Solution 1.

```cpp
// OJ: https://leetcode.com/problems/maximum-distance-in-arrays/
// Author: A M A N
// Time : O(N)
// Space: O(1)
class Solution {
public:
    int maxDistance(vector<vector<int>>& nums) {
        int mn1 = 1e9, mn2 = 1e9, mx1 = -1e9, mx2 = -1e9;
        int a = -1, b = -1, A = -1, B = -1;
        for(int i=0; i<nums.size(); i++){
            int last = nums[i].size()-1;
            //minimum
            if(nums[i][0]<mn1){
                mn2 = mn1;
                b = a;
                mn1 = nums[i][0];
                a = i;
            }else if(nums[i][0]<mn2){
                mn2 = nums[i][0];
                b = i;
            }
            //maximum
            if(nums[i][last]>mx1){
                mx2 = mx1;
                B = A;
                mx1 = nums[i][last];
                A = i;
            }else if(nums[i][last]>mx2){
                mx2 = nums[i][last];
                B = i;
            }
        }
        if(A!=a)    return mx1-mn1;
        else return max(mx1-mn2, mx2-mn1);
    }
};
```

without storing second max and second min;

```cpp
// OJ: https://leetcode.com/problems/maximum-distance-in-arrays/
// Author: A M A N
// Time : O(N)
// Space: O(1)
class Solution {
public:
    int maxDistance(vector<vector<int>>& A) {
        int mn = 1e9, mx = -1e9, ans = 0;
        for(int i=0; i<A.size(); i++){
            ans = max({ans, mx - A[i][0], A[i][A[i].size()-1] - mn});
            mn = min(mn, A[i][0]);
            mx = max(mx, A[i][A[i].size()-1]);
        }
        return ans;
    }
};
```