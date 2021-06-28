# [230. Kth Smallest Element in a BST (Medium)](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)

<p>Given the <code>root</code> of a binary search tree, and an integer <code>k</code>, return <em>the</em> <code>k<sup>th</sup></code> (<strong>1-indexed</strong>) <em>smallest element in the tree</em>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/01/28/kthtree1.jpg" style="width: 212px; height: 301px;">
<pre><strong>Input:</strong> root = [3,1,4,null,2], k = 1
<strong>Output:</strong> 1
</pre>

<p><strong>Example 2:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/01/28/kthtree2.jpg" style="width: 382px; height: 302px;">
<pre><strong>Input:</strong> root = [5,3,6,2,4,null,null,1], k = 3
<strong>Output:</strong> 3
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li>The number of nodes in the tree is <code>n</code>.</li>
	<li><code>1 &lt;= k &lt;= n &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= Node.val &lt;= 10<sup>4</sup></code></li>
</ul>

<p>&nbsp;</p>
<strong>Follow up:</strong> If the BST is modified often (i.e., we can do insert and delete operations) and you need to find the kth smallest frequently, how would you optimize?

**Related Topics**:  
[Tree](https://leetcode.com/tag/tree/), [Depth-First Search](https://leetcode.com/tag/depth-first-search/), [Binary Search Tree](https://leetcode.com/tag/binary-search-tree/), [Binary Tree](https://leetcode.com/tag/binary-tree/)

**Similar Questions**:
* [Binary Tree Inorder Traversal (Easy)](https://leetcode.com/problems/binary-tree-inorder-traversal/)
* [Second Minimum Node In a Binary Tree (Easy)](https://leetcode.com/problems/second-minimum-node-in-a-binary-tree/)

## Solution 1. DFS - Inorder Traversal

```cpp
// OJ: https://leetcode.com/problems/kth-smallest-element-in-a-bst/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    vector<int> v;
    void inorder(TreeNode* root){
        if(!root)
            return;
        inorder(root->left);
        v.push_back(root->val);
        inorder(root->right);
    }
    int kthSmallest(TreeNode* root, int k) {
        inorder(root);    
        return v[k-1];
    }
};
```

## Solution 2. Order Statistics in BST

Adding a count variable to each node to store the number of nodes in 
its subtree. Building takes O(N) but after that finding takes O(log N)

```cpp
// OJ: https://leetcode.com/problems/kth-smallest-element-in-a-bst/
// Author: A M A N
// Time : O(log N)
// Space: O(N)
class Solution {
public:
    class TreeNodeWithCount {
    public:
        int val;
        int count;
        TreeNodeWithCount* left;
        TreeNodeWithCount* right;
        TreeNodeWithCount(int x){
            val = x;
            count = 1; //size of the tree
            left = nullptr;
            right = nullptr;
        }
};
    // O(N)
    TreeNodeWithCount* buildTreeNodeWithCount(TreeNode* root){ }
    //O(log N)
    int find(TreeNodeWithCount* rootWithCount, int k){
        if(k<=0 or k>rootWithCount->count)
            return -1;
        if(rootWithCount->left){
            if(rootWithCount->left->count >=k ) 
                return find(rootWithCount->left, k);
            if(rootWithCount->left->count == k-1)
                return rootWithCount->val;
            return find(rootWithCount->right, k - rootWithCount->left->count - 1);
        }
        if(k==1)
            return rootWithCount->val;
        return find(rootWithCount->right, k-1);
    }
    int kthSmallest(TreeNode* root, int k) {
        TreeNodeWithCount* rootWithCount = buildTreeNodeWithCount(root);
        return find(rootWithCount, k);
    }
};
```