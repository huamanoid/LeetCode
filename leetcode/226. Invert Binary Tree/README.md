# [226. Invert Binary Tree (Easy)](https://leetcode.com/problems/invert-binary-tree/)

<p>Given the <code>root</code> of a binary tree, invert the tree, and return <em>its root</em>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/03/14/invert1-tree.jpg" style="width: 500px; height: 165px;">
<pre><strong>Input:</strong> root = [4,2,7,1,3,6,9]
<strong>Output:</strong> [4,7,2,9,6,3,1]
</pre>

<p><strong>Example 2:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/03/14/invert2-tree.jpg" style="width: 500px; height: 120px;">
<pre><strong>Input:</strong> root = [2,1,3]
<strong>Output:</strong> [2,3,1]
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> root = []
<strong>Output:</strong> []
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li>The number of nodes in the tree is in the range <code>[0, 100]</code>.</li>
	<li><code>-100 &lt;= Node.val &lt;= 100</code></li>
</ul>


**Related Topics**:  
[Tree](https://leetcode.com/tag/tree/), [Depth-First Search](https://leetcode.com/tag/depth-first-search/), [Breadth-First Search](https://leetcode.com/tag/breadth-first-search/), [Binary Tree](https://leetcode.com/tag/binary-tree/)

## Solution 1. Recursion

```cpp
// OJ: https://leetcode.com/problems/invert-binary-tree/
// Author: A M A N
// Time: O()
// Space: O()
class Solution {
public:
    Recursive Solution
    TreeNode* invertTree(TreeNode* root) {
        if(!root) return nullptr;
        swap(root->left, root->right);
        invertTree(root->left);
        invertTree(root->right);
        return root;
    }
};
```

## Solution 2. BFS

```cpp
// OJ: https://leetcode.com/problems/invert-binary-tree/
// Author: A M A N
// Time: O()
// Space: O()
class Solution {
public:
//     Iterative Solution
    TreeNode* invertTree(TreeNode* root){
        if(!root) return nullptr;
        stack<TreeNode*> st;
        st.push(root);
        while(st.size()){
            TreeNode* cur = st.top(); st.pop();
            if(!cur)continue;
            swap(cur->left, cur->right);
            st.push(cur->left);
            st.push(cur->right);
        }
        return root;
    }
};
```


