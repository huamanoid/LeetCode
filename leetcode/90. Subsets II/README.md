# [90. Subsets II (Medium)](https://leetcode.com/problems/subsets-ii/)

<p>Given an integer array <code>nums</code> that may contain duplicates, return <em>all possible subsets (the power set)</em>.</p>

<p>The solution set <strong>must not</strong> contain duplicate subsets. Return the solution in <strong>any order</strong>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<pre><strong>Input:</strong> nums = [1,2,2]
<strong>Output:</strong> [[],[1],[1,2],[1,2,2],[2],[2,2]]
</pre><p><strong>Example 2:</strong></p>
<pre><strong>Input:</strong> nums = [0]
<strong>Output:</strong> [[],[0]]
</pre>
<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10</code></li>
	<li><code>-10 &lt;= nums[i] &lt;= 10</code></li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Backtracking](https://leetcode.com/tag/backtracking/), [Bit Manipulation](https://leetcode.com/tag/bit-manipulation/)

**Similar Questions**:
* [Subsets (Medium)](https://leetcode.com/problems/subsets/)

## Solution 1. Backtracking

```cpp
// OJ: https://leetcode.com/problems/subsets-ii/
// Author: A M A N
//Time:  O(N^2 * 2^N)
//Space: O(N)
};
class Solution {
public:

    vector<vector<int>> ans;
    void solve(vector<int>& nums, int start, vector<int> temp){
        ans.push_back(temp);
        for(int i=start; i<nums.size(); i++){
            if(i and nums[i]==nums[i-1] and i>start) continue;
            temp.push_back(nums[i]);
            solve(nums, i+1, temp);
            temp.pop_back();
        }
        return;
    }
    vector<vector<int>> subsetsWithDup(vector<int>& nums){
        sort(nums.begin(), nums.end());
        solve(nums, 0, vector<int>());
        return ans;
    }
};
```

## Solution 2. Recursion

```cpp
// OJ: https://leetcode.com/problems/subsets-ii/
// Author: A M A N
//Time:  O(N^2 * 2^N)
//Space: O(N)
};
class Solution {
public:
    set<vector<int>> ans;
    void solve(vector<int>input, vector<int> output){
        if(input.size()==0){
            sort(output.begin(), output.end());
            ans.insert(output);
            return;
        }
        int temp = input.back();
        input.pop_back();
        solve(input, output);
        output.push_back(temp);
        solve(input, output);
        return;
    }
    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        vector<int> output;
        solve(nums, output);
        return vector<vector<int>> (ans.begin(), ans.end());
    }
};
```