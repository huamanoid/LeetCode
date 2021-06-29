# [897. Increasing Order Search Tree (Easy)](https://leetcode.com/problems/increasing-order-search-tree/)

<p>Given the <code>root</code> of a binary search tree, rearrange the tree in <strong>in-order</strong> so that the leftmost node in the tree is now the root of the tree, and every node has no left child and only one right child.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2020/11/17/ex1.jpg" style="width: 600px; height: 350px;">
<pre><strong>Input:</strong> root = [5,3,6,2,4,null,8,1,null,null,null,7,9]
<strong>Output:</strong> [1,null,2,null,3,null,4,null,5,null,6,null,7,null,8,null,9]
</pre>

<p><strong>Example 2:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2020/11/17/ex2.jpg" style="width: 300px; height: 114px;">
<pre><strong>Input:</strong> root = [5,1,7]
<strong>Output:</strong> [1,null,5,null,7]
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li>The number of nodes in the given tree will be in the range <code>[1, 100]</code>.</li>
	<li><code>0 &lt;= Node.val &lt;= 1000</code></li>
</ul>

**Related Topics**:  
[Stack](https://leetcode.com/tag/stack/), [Tree](https://leetcode.com/tag/tree/), [Depth-First Search](https://leetcode.com/tag/depth-first-search/), [Binary Search Tree](https://leetcode.com/tag/binary-search-tree/), [Binary Tree](https://leetcode.com/tag/binary-tree/)

## Solution 1. DFS

```cpp
// OJ: https://leetcode.com/problems/increasing-order-search-tree/
// Author: A M A N
// Time : O(N * log N)
// Space: O(N)
class Solution {
public:
    vector<int> inorder;
    void dfs(TreeNode* root){
        if(!root)
            return;
        dfs(root->left);
        inorder.push_back(root->val);
        dfs(root->right);
    }
    TreeNode* increasingBST(TreeNode* root) {
        dfs(root);
        sort(inorder.begin(), inorder.end());
        int idx = 0;
        TreeNode* r = new TreeNode(inorder[idx]);
        TreeNode* cur = r;
        while(++idx<inorder.size()){
            cur->left = nullptr;
            cur->right = new TreeNode(inorder[idx]);
            cur = cur->right;
        }
        cur->right = nullptr;
        return r;
    }
};
```