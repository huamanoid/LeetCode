# [565. Array Nesting (Medium)](https://leetcode.com/problems/array-nesting/)

<p>You are given an integer array <code>nums</code> of length <code>n</code> where <code>nums</code> is a permutation of the numbers in the range <code>[0, n - 1]</code>.</p>

<p>You should build a set <code>s[k] = {nums[k], nums[nums[k]], nums[nums[nums[k]]], ... }</code> subjected to the following rule:</p>

<ul>
	<li>The first element in <code>s[k]</code> starts with the selection of the element <code>nums[k]</code> of <code>index = k</code>.</li>
	<li>The next element in <code>s[k]</code> should be <code>nums[nums[k]]</code>, and then <code>nums[nums[nums[k]]]</code>, and so on.</li>
	<li>We stop adding right before a duplicate element occurs in <code>s[k]</code>.</li>
</ul>

<p>Return <em>the longest length of a set</em> <code>s[k]</code>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> nums = [5,4,0,3,1,6,2]
<strong>Output:</strong> 4
<strong>Explanation:</strong> 
nums[0] = 5, nums[1] = 4, nums[2] = 0, nums[3] = 3, nums[4] = 1, nums[5] = 6, nums[6] = 2.
One of the longest sets s[k]:
s[0] = {nums[0], nums[5], nums[6], nums[2]} = {5, 6, 2, 0}
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> nums = [0,1,2]
<strong>Output:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt; nums.length</code></li>
	<li>All the values of <code>nums</code> are <strong>unique</strong>.</li>
</ul>


**Companies**:  
[Adobe](https://leetcode.com/company/adobe)

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Depth-First Search](https://leetcode.com/tag/depth-first-search/)

**Similar Questions**:
* [Nested List Weight Sum (Medium)](https://leetcode.com/problems/nested-list-weight-sum/)
* [Flatten Nested List Iterator (Medium)](https://leetcode.com/problems/flatten-nested-list-iterator/)
* [Nested List Weight Sum II (Medium)](https://leetcode.com/problems/nested-list-weight-sum-ii/)

## Solution 1. DFS + Memoization


Without appreciating the beauty of the problem.

```cpp
// OJ: https://leetcode.com/problems/array-nesting/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    int dp[100001];
    int dfs(vector<int>&nums, int idx, vector<bool>&vis){
        if(dp[idx]!=-1) return dp[idx];
        if(vis[idx]) return 0;
        vis[idx] = true;
        return dp[idx] = 1 + dfs(nums, nums[idx], vis);
    }
    int arrayNesting(vector<int>& nums) {
        memset(dp, -1, sizeof dp);
        int ans = 0;         
        vector<bool> vis(nums.size(), false);
        for(int i=0; i<nums.size(); i++){
            ans = max(ans, dfs(nums, i, vis));
        }
        return ans;
    }
};
```



Now, let's appreciate the beauty of the problem.

![beauty](/assets/565.png)

So there exist `multiple` `independent` `complete` `cycles` (each node having 1 indegree and 1 outdegree) using any permutation.

Consequently if we `start` traversing from `any node` we'll `reach to itself`, if none of the element in the starting node's cycle is visited. 

And if a `node` turns out to be `visited` then it means the complete length of `the cycle` which the node is part of,  `has already been traversed` and length been calculated. So we can skip it.

```cpp
// OJ: https://leetcode.com/problems/array-nesting/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    int arrayNesting(vector<int>& nums) {
        int ans = 0;
        vector<bool>vis(nums.size(), false);
        for(int i=0; i<nums.size(); i++){
            int cnt = 0, cur = i;
            while(!vis[cur]){
                vis[cur] = true;
                cur = nums[cur];
                cnt++;
            }
            ans = max(ans, cnt);
        }
        return ans;
    }
};
```