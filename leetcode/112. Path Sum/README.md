# [112. Path Sum (Easy)](https://leetcode.com/problems/path-sum/)

<p>Given the <code>root</code> of a binary tree and an integer <code>targetSum</code>, return <code>true</code> if the tree has a <strong>root-to-leaf</strong> path such that adding up all the values along the path equals <code>targetSum</code>.</p>

<p>A <strong>leaf</strong> is a node with no children.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/01/18/pathsum1.jpg" style="width: 500px; height: 356px;">
<pre><strong>Input:</strong> root = [5,4,8,11,null,13,4,7,2,null,null,null,1], targetSum = 22
<strong>Output:</strong> true
</pre>

<p><strong>Example 2:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/01/18/pathsum2.jpg">
<pre><strong>Input:</strong> root = [1,2,3], targetSum = 5
<strong>Output:</strong> false
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> root = [1,2], targetSum = 0
<strong>Output:</strong> false
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li>The number of nodes in the tree is in the range <code>[0, 5000]</code>.</li>
	<li><code>-1000 &lt;= Node.val &lt;= 1000</code></li>
	<li><code>-1000 &lt;= targetSum &lt;= 1000</code></li>
</ul>


**Related Topics**:  
[Tree](https://leetcode.com/tag/tree/), [Depth-First Search](https://leetcode.com/tag/depth-first-search/), [Binary Tree](https://leetcode.com/tag/binary-tree/)

**Similar Questions**:
* [Path Sum II (Medium)](https://leetcode.com/problems/path-sum-ii/)
* [Binary Tree Maximum Path Sum (Hard)](https://leetcode.com/problems/binary-tree-maximum-path-sum/)
* [Sum Root to Leaf Numbers (Medium)](https://leetcode.com/problems/sum-root-to-leaf-numbers/)
* [Path Sum III (Medium)](https://leetcode.com/problems/path-sum-iii/)
* [Path Sum IV (Medium)](https://leetcode.com/problems/path-sum-iv/)

## Solution 1. Recursion

```cpp
// OJ: https://leetcode.com/problems/path-sum/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    bool hasPathSum(TreeNode* root, int targetSum) {
        if(!root) return false;
        if(!root->left and !root->right){
            return  root->val==targetSum;
        }
        bool l = hasPathSum(root->left, targetSum - root->val);
        bool r = hasPathSum(root->right, targetSum - root->val);
        return l or r;
    }
};
```