# [257. Binary Tree Paths (Easy)](https://leetcode.com/problems/binary-tree-paths/)

<p>Given the <code>root</code> of a binary tree, return <em>all root-to-leaf paths in <strong>any order</strong></em>.</p>

<p>A <strong>leaf</strong> is a node with no children.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/03/12/paths-tree.jpg" style="width: 207px; height: 293px;">
<pre><strong>Input:</strong> root = [1,2,3,null,5]
<strong>Output:</strong> ["1-&gt;2-&gt;5","1-&gt;3"]
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> root = [1]
<strong>Output:</strong> ["1"]
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li>The number of nodes in the tree is in the range <code>[1, 100]</code>.</li>
	<li><code>-100 &lt;= Node.val &lt;= 100</code></li>
</ul>


**Related Topics**:  
[String](https://leetcode.com/tag/string/), [Tree](https://leetcode.com/tag/tree/), [Depth-First Search](https://leetcode.com/tag/depth-first-search/), [Binary Tree](https://leetcode.com/tag/binary-tree/)

**Similar Questions**:
* [Path Sum II (Medium)](https://leetcode.com/problems/path-sum-ii/)
* [Smallest String Starting From Leaf (Medium)](https://leetcode.com/problems/smallest-string-starting-from-leaf/)

## Solution 1. DFS

```cpp
// OJ: https://leetcode.com/problems/binary-tree-paths/
// Author: A M A N
// Time: O(N)
// Space: O(N)
class Solution {
public:
    vector<string> ans;
    void dfs(TreeNode* root, string temp){
        if(!root)
            return;
        if(!root->left and !root->right){
            temp += to_string(root->val);
            ans.push_back(temp);
            return;
        }
        temp +=  to_string(root->val) + "->";
        dfs(root->left, temp);
        dfs(root->right, temp);
        return;
    }
    vector<string> binaryTreePaths(TreeNode* root) {
        dfs(root, "");
        return ans;
    }
};
```