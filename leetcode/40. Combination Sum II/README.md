# [40. Combination Sum II (Medium)](https://leetcode.com/problems/combination-sum-ii/)

<p>Given a collection of candidate numbers (<code>candidates</code>) and a target number (<code>target</code>), find all unique combinations in <code>candidates</code>&nbsp;where the candidate numbers sum to <code>target</code>.</p>

<p>Each number in <code>candidates</code>&nbsp;may only be used <strong>once</strong> in the combination.</p>

<p><strong>Note:</strong>&nbsp;The solution set must not contain duplicate combinations.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> candidates = [10,1,2,7,6,1,5], target = 8
<strong>Output:</strong> 
[
[1,1,6],
[1,2,5],
[1,7],
[2,6]
]
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> candidates = [2,5,2,1,2], target = 5
<strong>Output:</strong> 
[
[1,2,2],
[5]
]
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;=&nbsp;candidates.length &lt;= 100</code></li>
	<li><code>1 &lt;=&nbsp;candidates[i] &lt;= 50</code></li>
	<li><code>1 &lt;= target &lt;= 30</code></li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Backtracking](https://leetcode.com/tag/backtracking/)

**Similar Questions**:
* [Combination Sum (Medium)](https://leetcode.com/problems/combination-sum/)

## Solution 1. Backtracking

```cpp
// OJ: https://leetcode.com/problems/combination-sum-ii/
// Author: A M A N
//Time:  O(N^2)
//Space: O(N)
class Solution {
public:

    vector<vector<int>> ans;
    void solve(vector<int>& candidates, int start, int sum, int target, vector<int> temp){
        if(sum==target){
            ans.push_back(temp);
            return;
        }
        for(int i=start; i<candidates.size(); i++){
//             VERY IMPORTANT STEP TO VISUALIZE AND UNDERSTAND
            // at each position, elements with duplicates are inserted only once.
            // i>start step happens only after the for loop runs again after one call, (aka. after pushing, calling and POPING and after next iteration if you call again for same number as prev )and if the element is same for which 
            //     one call had already been called, this will yield in repeated answers.
            if(i and candidates[i]==candidates[i-1] and i>start) continue;
            if(candidates[i]+sum<=target){
                temp.push_back(candidates[i]);
                solve(candidates, i+1, sum+candidates[i], target, temp);
                temp.pop_back();
            }
        }
        return;
    }
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        vector<int> temp;
        sort(candidates.begin(), candidates.end());
        solve(candidates, 0, 0, target, temp);
        return ans;
    }
};
```