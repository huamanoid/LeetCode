# [298. Binary Tree Longest Consecutive Sequence (Medium)](https://leetcode.com/problems/binary-tree-longest-consecutive-sequence/)

<p>Given the <code>root</code> of a binary tree, return <em>the length of the longest consecutive sequence path</em>.</p>

<p>The path refers to any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The longest consecutive path needs to be from parent to child (cannot be the reverse).</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/03/14/consec1-1-tree.jpg" style="width: 322px; height: 421px;">
<pre><strong>Input:</strong> root = [1,null,3,2,4,null,null,null,5]
<strong>Output:</strong> 3
<strong>Explanation:</strong> Longest consecutive sequence path is 3-4-5, so return 3.
</pre>

<p><strong>Example 2:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/03/14/consec1-2-tree.jpg" style="width: 262px; height: 421px;">
<pre><strong>Input:</strong> root = [2,null,3,2,null,1]
<strong>Output:</strong> 2
<strong>Explanation:</strong> Longest consecutive sequence path is 2-3, not 3-2-1, so return 2.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li>The number of nodes in the tree is in the range <code>[1, 3 * 10<sup>4</sup>]</code>.</li>
	<li><code>-3 * 10<sup>4</sup> &lt;= Node.val &lt;= 3 * 10<sup>4</sup></code></li>
</ul>


**Companies**:  
[ByteDance](https://leetcode.com/company/bytedance)

**Related Topics**:  
[Tree](https://leetcode.com/tag/tree/), [Depth-First Search](https://leetcode.com/tag/depth-first-search/), [Binary Tree](https://leetcode.com/tag/binary-tree/)

**Similar Questions**:
* [Longest Consecutive Sequence (Medium)](https://leetcode.com/problems/longest-consecutive-sequence/)
* [Binary Tree Longest Consecutive Sequence II (Medium)](https://leetcode.com/problems/binary-tree-longest-consecutive-sequence-ii/)

## Solution 1. Pre-order Traversal

```cpp
// OJ: https://leetcode.com/problems/binary-tree-longest-consecutive-sequence/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    int ans;
    void solve(TreeNode* root, int par, int len){
        if(!root) return;
        if(root->val==par+1) len++;
        else len = 1;
        ans = max(ans, len);
        solve(root->left, root->val, len);
        solve(root->right, root->val, len);
    }
    int longestConsecutive(TreeNode* root) {
        solve(root, INT_MIN, 0);
        return ans;
    }
};
```
without using a global variable

```cpp
// OJ: https://leetcode.com/problems/binary-tree-longest-consecutive-sequence/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    int solve(TreeNode* root, int par, int len){
        if(!root) return len;
        if(root->val==par+1) len++;
        else len = 1;
        int l = solve(root->left, root->val, len);
        int r = solve(root->right, root->val, len);
        return max(len, max(l, r));
    }
    int longestConsecutive(TreeNode* root) {
        return solve(root, INT_MIN, 0);
    }
};
```

## Solution 2. Post-order Traversal
```cpp
// OJ: https://leetcode.com/problems/binary-tree-longest-consecutive-sequence/
// Author: A M A N
// Time : O()
// Space: O()
class Solution {
public:
    int ans;
    int solve(TreeNode* root){
        if(!root) return 0;
        int l = 1 + solve(root->left);
        int r = 1 + solve(root->right);
        if(root->left and root->val+1!=root->left->val)
            l = 1;
        if(root->right and root->val+1!=root->right->val)
            r = 1;
        ans = max(ans, max(l, r));
        return max(l, r);
    }
    int longestConsecutive(TreeNode* root) {
        solve(root);
        return ans;
    }
};
```