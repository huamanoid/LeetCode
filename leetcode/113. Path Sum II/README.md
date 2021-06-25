# [113. Path Sum II (Medium)](https://leetcode.com/problems/path-sum-ii/)

<p>Given the <code>root</code> of a binary tree and an integer <code>targetSum</code>, return all <strong>root-to-leaf</strong> paths where each path's sum equals <code>targetSum</code>.</p>

<p>A <strong>leaf</strong> is a node with no children.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/01/18/pathsumii1.jpg" style="width: 500px; height: 356px;">
<pre><strong>Input:</strong> root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22
<strong>Output:</strong> [[5,4,11,2],[5,8,4,5]]
</pre>

<p><strong>Example 2:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/01/18/pathsum2.jpg" style="width: 212px; height: 181px;">
<pre><strong>Input:</strong> root = [1,2,3], targetSum = 5
<strong>Output:</strong> []
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> root = [1,2], targetSum = 0
<strong>Output:</strong> []
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li>The number of nodes in the tree is in the range <code>[0, 5000]</code>.</li>
	<li><code>-1000 &lt;= Node.val &lt;= 1000</code></li>
	<li><code>-1000 &lt;= targetSum &lt;= 1000</code></li>
</ul>


**Related Topics**:  
[Backtracking](https://leetcode.com/tag/backtracking/), [Tree](https://leetcode.com/tag/tree/), [Depth-First Search](https://leetcode.com/tag/depth-first-search/), [Binary Tree](https://leetcode.com/tag/binary-tree/)

**Similar Questions**:
* [Path Sum (Easy)](https://leetcode.com/problems/path-sum/)
* [Binary Tree Paths (Easy)](https://leetcode.com/problems/binary-tree-paths/)
* [Path Sum III (Medium)](https://leetcode.com/problems/path-sum-iii/)
* [Path Sum IV (Medium)](https://leetcode.com/problems/path-sum-iv/)

## Solution 1. DFS

```cpp
// OJ: https://leetcode.com/problems/path-sum-ii/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    vector<vector<int>>ans;
    void dfs(TreeNode* u, int target, vector<int> temp){
        if(!u) return;
        if(!u->left and !u->right){
            if(target==u->val){
                temp.push_back(u->val);
                ans.push_back(temp);
                return;
            }
        }
        temp.push_back(u->val);
        dfs(u->left, target-u->val, temp);
        dfs(u->right, target-u->val, temp);
    }
    vector<vector<int>> pathSum(TreeNode* root, int targetSum) {
        dfs(root, targetSum, vector<int>());
        return ans;
    }
};
```