# [1382. Balance a Binary Search Tree (Medium)](https://leetcode.com/problems/balance-a-binary-search-tree/)

<p>Given the <code>root</code> of a binary search tree, return <em>a <strong>balanced</strong> binary search tree with the same node values</em>. If there is more than one answer, return <strong>any of them</strong>.</p>

<p>A binary search tree is <strong>balanced</strong> if the depth of the two subtrees of every node never differs by more than <code>1</code>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/08/10/balance1-tree.jpg" style="width: 500px; height: 319px;">
<pre><strong>Input:</strong> root = [1,null,2,null,3,null,4,null,null]
<strong>Output:</strong> [2,1,3,null,null,null,4]
<b>Explanation:</b> This is not the only correct answer, [3,1,4,null,2] is also correct.
</pre>

<p><strong>Example 2:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/08/10/balanced2-tree.jpg" style="width: 224px; height: 145px;">
<pre><strong>Input:</strong> root = [2,1,3]
<strong>Output:</strong> [2,1,3]
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li>The number of nodes in the tree is in the range <code>[1, 10<sup>4</sup>]</code>.</li>
	<li><code>1 &lt;= Node.val &lt;= 10<sup>5</sup></code></li>
</ul>


**Companies**:  
[Facebook](https://leetcode.com/company/facebook), [Amazon](https://leetcode.com/company/amazon), [Goldman Sachs](https://leetcode.com/company/goldman-sachs)

**Related Topics**:  
[Divide and Conquer](https://leetcode.com/tag/divide-and-conquer/), [Greedy](https://leetcode.com/tag/greedy/), [Tree](https://leetcode.com/tag/tree/), [Depth-First Search](https://leetcode.com/tag/depth-first-search/), [Binary Search Tree](https://leetcode.com/tag/binary-search-tree/), [Binary Tree](https://leetcode.com/tag/binary-tree/)

## Solution 1. In-order + Post-order

```cpp
// OJ: https://leetcode.com/problems/balance-a-binary-search-tree/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    void inorder(TreeNode* root, vector<int>&v){
        if(!root)return;
        inorder(root->left, v);
        v.push_back(root->val);
        inorder(root->right, v);
    }
    TreeNode* constructBST(vector<int>&v, int start, int end){
        if(start>end) return nullptr;
        int mid = start + (end-start)/2;
        auto l = constructBST(v, start, mid-1);
        auto r = constructBST(v, mid+1, end);
        TreeNode* root = new TreeNode(v[mid]);
        root->left = l;
        root->right = r;
        return root;
    }
    TreeNode* balanceBST(TreeNode* root) {
        vector<int> v;
        inorder(root, v);
        return constructBST(v, 0, v.size()-1);
    }
};
```