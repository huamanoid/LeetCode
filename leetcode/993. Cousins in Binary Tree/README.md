# [993. Cousins in Binary Tree (Easy)](https://leetcode.com/problems/cousins-in-binary-tree/)

<p>In a binary tree, the root node is at depth <code>0</code>, and children of each depth <code>k</code> node are at depth <code>k+1</code>.</p>

<p>Two nodes of a binary tree are <em>cousins</em> if they have the same depth, but have <strong>different parents</strong>.</p>

<p>We are given the <code>root</code> of a binary tree with unique values, and the values <code>x</code>&nbsp;and <code>y</code>&nbsp;of two different nodes in the tree.</p>

<p>Return&nbsp;<code>true</code>&nbsp;if and only if the nodes corresponding to the values <code>x</code> and <code>y</code> are cousins.</p>

<p>&nbsp;</p>

<p><strong>Example 1:<br>
<img alt="" src="https://assets.leetcode.com/uploads/2019/02/12/q1248-01.png" style="width: 180px; height: 160px;"></strong></p>

<pre><strong>Input: </strong>root = <span id="example-input-1-1">[1,2,3,4]</span>, x = <span id="example-input-1-2">4</span>, y = <span id="example-input-1-3">3</span>
<strong>Output: </strong><span id="example-output-1">false</span>
</pre>

<div>
<p><strong>Example 2:<br>
<img alt="" src="https://assets.leetcode.com/uploads/2019/02/12/q1248-02.png" style="width: 201px; height: 160px;"></strong></p>

<pre><strong>Input: </strong>root = <span id="example-input-2-1">[1,2,3,null,4,null,5]</span>, x = <span id="example-input-2-2">5</span>, y = <span id="example-input-2-3">4</span>
<strong>Output: </strong><span id="example-output-2">true</span>
</pre>

<div>
<p><strong>Example 3:</strong></p>

<p><strong><img alt="" src="https://assets.leetcode.com/uploads/2019/02/13/q1248-03.png" style="width: 156px; height: 160px;"></strong></p>

<pre><strong>Input: </strong>root = <span id="example-input-3-1">[1,2,3,null,4]</span>, x = 2, y = 3
<strong>Output: </strong><span id="example-output-3">false</span>
</pre>
</div>
</div>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li>The number of nodes in the tree will be between <code>2</code> and <code>100</code>.</li>
	<li>Each node has a unique integer value from <code>1</code> to <code>100</code>.</li>
</ul>


**Related Topics**:  
[Tree](https://leetcode.com/tag/tree/), [Depth-First Search](https://leetcode.com/tag/depth-first-search/), [Breadth-First Search](https://leetcode.com/tag/breadth-first-search/), [Binary Tree](https://leetcode.com/tag/binary-tree/)

**Similar Questions**:
* [Binary Tree Level Order Traversal (Medium)](https://leetcode.com/problems/binary-tree-level-order-traversal/)

## Solution 1. DFS

```cpp
// OJ: https://leetcode.com/problems/cousins-in-binary-tree/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    TreeNode* xpar, *ypar;
    int xdepth, ydepth;
    void dfs(TreeNode* root, TreeNode* par, int d,  int x, int y){
        if(!root)
            return;
        if(root->val==x){
            xpar = par;
            xdepth = d;
        }
        if(root->val==y){
            ypar = par;
            ydepth = d;
        }
        dfs(root->left, root, d+1, x, y);
        dfs(root->right, root, d+1, x, y);
    }
    bool isCousins(TreeNode* root, int x, int y) {
        dfs(root, nullptr, 0, x, y);
        if(xpar==ypar or ydepth!=xdepth)
            return false;
        return true;
    }
};
```