# [863. All Nodes Distance K in Binary Tree (Medium)](https://leetcode.com/problems/all-nodes-distance-k-in-binary-tree/)

<p>We are given a binary tree (with root node&nbsp;<code>root</code>), a <code>target</code> node, and an integer value <code>k</code>.</p>

<p>Return a list of the values of all&nbsp;nodes that have a distance <code>k</code> from the <code>target</code> node.&nbsp; The answer can be returned in any order.</p>

<p>&nbsp;</p>

<ol>
</ol>

<div>
<p><strong>Example 1:</strong></p>

<pre><strong>Input: </strong>root = <span id="example-input-1-1">[3,5,1,6,2,0,8,null,null,7,4]</span>, target = <span id="example-input-1-2">5</span>, k = <span id="example-input-1-3">2</span>

<strong>Output: </strong><span id="example-output-1">[7,4,1]</span>

<strong>Explanation: </strong>
The nodes that are a distance 2 from the target node (with value 5)
have values 7, 4, and 1.

<img alt="" src="https://s3-lc-upload.s3.amazonaws.com/uploads/2018/06/28/sketch0.png" style="width: 280px; height: 240px;">

Note that the inputs "root" and "target" are actually TreeNodes.
The descriptions of the inputs above are just serializations of these objects.
</pre>

<p>&nbsp;</p>

<p><strong>Note:</strong></p>

<ol>
	<li>The given tree is non-empty.</li>
	<li>Each node in the tree has unique values&nbsp;<code>0 &lt;= node.val &lt;= 500</code>.</li>
	<li>The <code>target</code>&nbsp;node is a node in the tree.</li>
	<li><code>0 &lt;= k &lt;= 1000</code>.</li>
</ol>
</div>


**Related Topics**:  
[Tree](https://leetcode.com/tag/tree/), [Depth-First Search](https://leetcode.com/tag/depth-first-search/), [Breadth-First Search](https://leetcode.com/tag/breadth-first-search/), [Binary Tree](https://leetcode.com/tag/binary-tree/)

## Solution 1. DFS

```cpp
// OJ: https://leetcode.com/problems/all-nodes-distance-k-in-binary-tree/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    unordered_map<TreeNode*, TreeNode*> p;
    unordered_set<TreeNode*> vis;
    vector<int> ans;
    void dfs(TreeNode* u, int k){
        if(!u) return;
        if(vis.find(u)!=vis.end()) return;
        vis.insert(u); //since we're traversing up and down from the same node, u -> parent[u] -> parent[u]->left or right may drop us back and may cause repitition or TLE so we need visited
        if(k==0){
            ans.push_back(u->val);
            return;
        }
        dfs(p[u], k-1); // just an extra recursion for traversing through parent along with the children
        dfs(u->left, k-1);
        dfs(u->right, k-1);
    }
    void buildParent(TreeNode* u, TreeNode* pu){
        if(!u)
            return;
        p[u] = pu;
        buildParent(u->left, u);
        buildParent(u->right, u);
    }
    vector<int> distanceK(TreeNode* root, TreeNode* target, int k) {
        buildParent(root, nullptr);
        dfs(target, k);
        return ans;
    }
};
```