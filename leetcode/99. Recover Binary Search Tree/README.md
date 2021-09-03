# [99. Recover Binary Search Tree (Medium)](https://leetcode.com/problems/recover-binary-search-tree/)

<p>You are given the <code>root</code> of a binary search tree (BST), where exactly two nodes of the tree were swapped by mistake. <em>Recover the tree without changing its structure</em>.</p>

<p><strong>Follow up:</strong> A solution using <code>O(n)</code> space is pretty straight forward. Could you devise a constant space solution?</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2020/10/28/recover1.jpg" style="width: 422px; height: 302px;">
<pre><strong>Input:</strong> root = [1,3,null,null,2]
<strong>Output:</strong> [3,1,null,null,2]
<strong>Explanation:</strong> 3 cannot be a left child of 1 because 3 &gt; 1. Swapping 1 and 3 makes the BST valid.
</pre>

<p><strong>Example 2:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2020/10/28/recover2.jpg" style="width: 581px; height: 302px;">
<pre><strong>Input:</strong> root = [3,1,4,null,null,2]
<strong>Output:</strong> [2,1,4,null,null,3]
<strong>Explanation:</strong> 2 cannot be in the right subtree of 3 because 2 &lt; 3. Swapping 2 and 3 makes the BST valid.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li>The number of nodes in the tree is in the range <code>[2, 1000]</code>.</li>
	<li><code>-2<sup>31</sup> &lt;= Node.val &lt;= 2<sup>31</sup> - 1</code></li>
</ul>


**Related Topics**:  
[Tree](https://leetcode.com/tag/tree/), [Depth-First Search](https://leetcode.com/tag/depth-first-search/), [Binary Search Tree](https://leetcode.com/tag/binary-search-tree/), [Binary Tree](https://leetcode.com/tag/binary-tree/)


## Solution 1. Inorder traversal
```cpp
// OJ: https://leetcode.com/problems/recover-binary-search-tree/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    TreeNode* found[2] = {nullptr, nullptr};
    TreeNode* prev = new TreeNode(INT_MIN);
    void inorder(TreeNode* root){
        if(!root)return;
        inorder(root->left);
        if(found[0]==nullptr and prev->val > root->val)
            found[0] = prev;
        if(found[0] and prev->val > root->val)
            found[1] = root;
        prev = root;
        inorder(root->right);
    }
    void recoverTree(TreeNode* root) {
        inorder(root); //inorder traversal of *BST* happens in a sorted fashion        
        swap(found[0]->val, found[1]->val);
        return;
    }
};
```


## Solution 2.
Comparing the inorder with the sorted to find the swapped elements
```cpp
// OJ: https://leetcode.com/problems/recover-binary-search-tree/
// Author: A M A N
// Time : O(N * log N)
// Space: O(N)
class Solution {
public:
    vector<TreeNode*> v;
    void inorder(TreeNode* root){
        if(!root)return;
        inorder(root->left);
        v.push_back(root);
        inorder(root->right);
    }
    void recoverTree(TreeNode* root) {
        inorder(root); //inorder traversal of *BST* happens in a sorted fashion        
        vector<int>temp;
        for(auto a: v)  temp.push_back(a->val);
        sort(temp.begin(), temp.end());
        //sort and find the two swapped nodes.
        vector<TreeNode*> found;
        for(int i=0; i<temp.size(); i++)
            if(v[i]->val != temp[i])
                found.push_back(v[i]);
        swap(found[0]->val, found[1]->val);
        return;
    }
};
```