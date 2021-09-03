# [47. Permutations II (Medium)](https://leetcode.com/problems/permutations-ii/)

<p>Given a collection of numbers, <code>nums</code>,&nbsp;that might contain duplicates, return <em>all possible unique permutations <strong>in any order</strong>.</em></p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> nums = [1,1,2]
<strong>Output:</strong>
[[1,1,2],
 [1,2,1],
 [2,1,1]]
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> nums = [1,2,3]
<strong>Output:</strong> [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 8</code></li>
	<li><code>-10 &lt;= nums[i] &lt;= 10</code></li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Backtracking](https://leetcode.com/tag/backtracking/)

**Similar Questions**:
* [Next Permutation (Medium)](https://leetcode.com/problems/next-permutation/)
* [Permutations (Medium)](https://leetcode.com/problems/permutations/)
* [Palindrome Permutation II (Medium)](https://leetcode.com/problems/palindrome-permutation-ii/)
* [Number of Squareful Arrays (Hard)](https://leetcode.com/problems/number-of-squareful-arrays/)

## Solution 1. Backtracking

```cpp
// OJ: https://leetcode.com/problems/permutations-ii/
// Author: A M A N
//Time:  O(N * N!)
//Space: O(N)
class Solution {
public:

    vector<vector<int>> ans;
    void solve(vector<int>& nums, int start,  vector<bool>vis, vector<int> temp){
        if(temp.size()==nums.size()){
            ans.push_back(temp);
            return;
        }
        for(int i = 0; i<nums.size(); i++){
            if(i and nums[i]==nums[i-1] and !vis[i-1]) continue;
            if(!vis[i]){
                vis[i] = 1;
                temp.push_back(nums[i]);
                solve(nums, i, vis, temp);
                temp.pop_back();
                vis[i] = 0;
            }
        }
        return;
    }
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        vector<bool> vis(nums.size(), false);
        solve(nums,0, vis, vector<int>());
        return ans;
    }
};
```