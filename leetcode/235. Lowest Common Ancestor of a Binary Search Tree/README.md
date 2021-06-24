# [235. Lowest Common Ancestor of a Binary Search Tree (Easy)](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)

<p>Given a binary search tree (BST), find the lowest common ancestor (LCA) of two given nodes in the BST.</p>

<p>According to the <a href="https://en.wikipedia.org/wiki/Lowest_common_ancestor" target="_blank">definition of LCA on Wikipedia</a>: “The lowest common ancestor is defined between two nodes <code>p</code> and <code>q</code> as the lowest node in <code>T</code> that has both <code>p</code> and <code>q</code> as descendants (where we allow <b>a node to be a descendant of itself</b>).”</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2018/12/14/binarysearchtree_improved.png" style="width: 200px; height: 190px;">
<pre><strong>Input:</strong> root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
<strong>Output:</strong> 6
<strong>Explanation:</strong> The LCA of nodes 2 and 8 is 6.
</pre>

<p><strong>Example 2:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2018/12/14/binarysearchtree_improved.png" style="width: 200px; height: 190px;">
<pre><strong>Input:</strong> root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4
<strong>Output:</strong> 2
<strong>Explanation:</strong> The LCA of nodes 2 and 4 is 2, since a node can be a descendant of itself according to the LCA definition.
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> root = [2,1], p = 2, q = 1
<strong>Output:</strong> 2
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li>The number of nodes in the tree is in the range <code>[2, 10<sup>5</sup>]</code>.</li>
	<li><code>-10<sup>9</sup> &lt;= Node.val &lt;= 10<sup>9</sup></code></li>
	<li>All <code>Node.val</code> are <strong>unique</strong>.</li>
	<li><code>p != q</code></li>
	<li><code>p</code> and <code>q</code> will exist in the BST.</li>
</ul>


**Related Topics**:  
[Tree](https://leetcode.com/tag/tree/), [Depth-First Search](https://leetcode.com/tag/depth-first-search/), [Binary Search Tree](https://leetcode.com/tag/binary-search-tree/), [Binary Tree](https://leetcode.com/tag/binary-tree/)

**Similar Questions**:
* [Lowest Common Ancestor of a Binary Tree (Medium)](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)
* [Smallest Common Region (Medium)](https://leetcode.com/problems/smallest-common-region/)
* [Lowest Common Ancestor of a Binary Tree II (Medium)](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree-ii/)
* [Lowest Common Ancestor of a Binary Tree III (Medium)](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree-iii/)
* [Lowest Common Ancestor of a Binary Tree IV (Medium)](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree-iv/)

## Solution 1. Naive Approach


```cpp
// OJ: https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    unordered_map<TreeNode*, TreeNode*> parent;
    void constructParent(TreeNode* root){
        stack<pair<TreeNode*, TreeNode*>> st;
        st.push({root, parent[root]});
        while(st.size()){
            auto [u, pu] = st.top(); st.pop();
            parent[u] = pu;
            if(u->left)
                st.push({u->left, u});
            if(u->right)
                st.push({u->right, u});
        }
    }

    TreeNode* up(TreeNode* root, int cnt){
        while(cnt--){
            root = parent[root];
        }
        return root;
    }

    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        int pDepth = -1, qDepth = -1;
        stack<pair<TreeNode*, int>> st;
        st.push({root, 0});
        while(st.size()){
            auto [t, level] = st.top(); st.pop();
            if(!t) continue;
            if(t==p) pDepth = level;
            if(t==q) qDepth = level;
            st.push({t->left, level+1});
            st.push({t->right, level+1});
        }
        constructParent(root);
        int diff = pDepth - qDepth;
        // Lifting both of the nodes to a common level
        if(diff>0)    
            p = up(p, diff);
        else if(diff<0)
            q = up(q, -diff);

        while(p!=q){
            p = parent[p];
            q = parent[q];
        }
        return p;
    }
};
```

## Solution 2. Recursion
Using the fact that the tree is a BST

```cpp
// OJ: https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        int pVal = p->val, qVal = q->val;
        int node = root->val;
        if(pVal> node and qVal>node)
            return lowestCommonAncestor(root->right, p, q);
        else if(pVal< node and qVal< node)
            return lowestCommonAncestor(root->left, p, q);
        else
            return root;
    }
};
```