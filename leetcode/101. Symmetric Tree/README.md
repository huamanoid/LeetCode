# [101. Symmetric Tree (Easy)](https://leetcode.com/problems/symmetric-tree/)

<p>Given the <code>root</code> of a binary tree, <em>check whether it is a mirror of itself</em> (i.e., symmetric around its center).</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/02/19/symtree1.jpg" style="width: 354px; height: 291px;">
<pre><strong>Input:</strong> root = [1,2,2,3,4,4,3]
<strong>Output:</strong> true
</pre>

<p><strong>Example 2:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/02/19/symtree2.jpg" style="width: 308px; height: 258px;">
<pre><strong>Input:</strong> root = [1,2,2,null,3,null,3]
<strong>Output:</strong> false
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li>The number of nodes in the tree is in the range <code>[1, 1000]</code>.</li>
	<li><code>-100 &lt;= Node.val &lt;= 100</code></li>
</ul>

<p>&nbsp;</p>
<strong>Follow up:</strong> Could you solve it both recursively and iteratively?

**Related Topics**:  
[Tree](https://leetcode.com/tag/tree/), [Depth-First Search](https://leetcode.com/tag/depth-first-search/), [Breadth-First Search](https://leetcode.com/tag/breadth-first-search/), [Binary Tree](https://leetcode.com/tag/binary-tree/)

## Solution 1. Recursion

```cpp
// OJ: https://leetcode.com/problems/symmetric-tree/
// Author: A M A N
// Time: O(N)
// Space: O(N)
class Solution {
public:
    bool same(TreeNode* A, TreeNode* B){
        if(!A and !B) return true;
        if(!A ^ !B) return false;
        if(A->val != B->val)
            return false;
        //comparing the left child of first with the right child of second.
        return same(A->left, B->right) and same(A->right, B->left);
    }
    bool isSymmetric(TreeNode* root) {
        return same(root->left, root->right);
    }
};
```

## Solution 2. Iterative
Notice the order of pushing the nodes into stack.
```cpp
// OJ: https://leetcode.com/problems/symmetric-tree/
// Author: A M A N
// Time: O(N)
// Space: O(N)
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        stack<TreeNode*> st;
        st.push(root->left);
        st.push(root->right);
        while(!st.empty()){
            TreeNode* t1 = st.top(); st.pop();
            TreeNode* t2 = st.top(); st.pop();
            if(!t1 and !t2) continue;
            if(!t1 ^ !t2) return false;
            if(t1->val != t2->val) return false;
            st.push(t1->left);
            st.push(t2->right);
            st.push(t1->right);
            st.push(t2->left);
        }
        return true;
    }
};
```