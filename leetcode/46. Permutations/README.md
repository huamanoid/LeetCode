# [46. Permutations (Medium)](https://leetcode.com/problems/permutations/)

<p>Given an array <code>nums</code> of distinct integers, return <em>all the possible permutations</em>. You can return the answer in <strong>any order</strong>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<pre><strong>Input:</strong> nums = [1,2,3]
<strong>Output:</strong> [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
</pre><p><strong>Example 2:</strong></p>
<pre><strong>Input:</strong> nums = [0,1]
<strong>Output:</strong> [[0,1],[1,0]]
</pre><p><strong>Example 3:</strong></p>
<pre><strong>Input:</strong> nums = [1]
<strong>Output:</strong> [[1]]
</pre>
<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 6</code></li>
	<li><code>-10 &lt;= nums[i] &lt;= 10</code></li>
	<li>All the integers of <code>nums</code> are <strong>unique</strong>.</li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Backtracking](https://leetcode.com/tag/backtracking/)

**Similar Questions**:
* [Next Permutation (Medium)](https://leetcode.com/problems/next-permutation/)
* [Permutations II (Medium)](https://leetcode.com/problems/permutations-ii/)
* [Permutation Sequence (Hard)](https://leetcode.com/problems/permutation-sequence/)
* [Combinations (Medium)](https://leetcode.com/problems/combinations/)

## Solution 1. Backtracking

```cpp
// OJ: https://leetcode.com/problems/permutations/
// Author: A M A N
// Time: O(N!)
// Space: O(N)
class Solution {
public:
    vector<vector<int>> ans;
    void solve(vector<int>& nums, vector<bool>vis, vector<int> temp){
        if(temp.size()==nums.size()){
            ans.push_back(temp);
            return;
        }
        for(int i = 0; i<nums.size(); i++){
            if(!vis[i]){
                vis[i] = 1;
                temp.push_back(nums[i]);
                solve(nums, vis, temp);
                temp.pop_back();
                vis[i] = 0;
            }
        }
        return;
    }
    vector<vector<int>> permute(vector<int>& nums) {
        vector<bool> vis(nums.size(), false);
        solve(nums, vis, vector<int>());
        return ans;
    }
};
```