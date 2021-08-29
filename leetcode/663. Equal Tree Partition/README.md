# [663. Equal Tree Partition (Medium)](https://leetcode.com/problems/equal-tree-partition/)

<p>Given the <code>root</code> of a binary tree, return <code>true</code><em> if you can partition the tree into two trees with equal sums of values after removing exactly one edge on the original tree</em>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/05/03/split1-tree.jpg" style="width: 500px; height: 204px;">
<pre><strong>Input:</strong> root = [5,10,10,null,null,2,3]
<strong>Output:</strong> true
</pre>

<p><strong>Example 2:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/05/03/split2-tree.jpg" style="width: 277px; height: 302px;">
<pre><strong>Input:</strong> root = [1,2,10,null,null,2,20]
<strong>Output:</strong> false
<strong>Explanation:</strong> You cannot split the tree into two trees with equal sums after removing exactly one edge on the tree.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li>The number of nodes in the tree is in the range <code>[1, 10<sup>4</sup>]</code>.</li>
	<li><code>-10<sup>5</sup> &lt;= Node.val &lt;= 10<sup>5</sup></code></li>
</ul>


**Companies**:  
[Amazon](https://leetcode.com/company/amazon)

**Related Topics**:  
[Tree](https://leetcode.com/tag/tree/), [Depth-First Search](https://leetcode.com/tag/depth-first-search/), [Binary Tree](https://leetcode.com/tag/binary-tree/)

## Solution 1. DFS

```cpp
// OJ: https://leetcode.com/problems/equal-tree-partition/
// Author: A M A N
// Time : O(N)
// Space: O(H)
class Solution {
public:
    int sum;
    int calculateSum(TreeNode* root){
        if(!root)
            return 0;
        int l = calculateSum(root->left);
        int r = calculateSum(root->right);
        return l+r+root->val;
    }
    bool can = false;
    int dfs(TreeNode* root){
        if(!root)return 0;
        int l = dfs(root->left);
        int r = dfs(root->right);
        if((root->right and r==sum/2) or (root->left and l==sum/2)){
            can = true;
        }
        return l+r+root->val;
    }
    bool checkEqualTree(TreeNode* root) {
        sum = calculateSum(root);  
        if(sum&1 )return false;
        dfs(root);
        return can;
    }
};
```