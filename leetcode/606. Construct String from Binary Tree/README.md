# [606. Construct String from Binary Tree (Easy)](https://leetcode.com/problems/construct-string-from-binary-tree/)

<p>Given the <code>root</code> of a binary tree, construct a string consists of parenthesis and integers from a binary tree with the preorder traversing way, and return it.</p>

<p>Omit all the empty parenthesis pairs that do not affect the one-to-one mapping relationship between the string and the original binary tree.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/05/03/cons1-tree.jpg" style="width: 292px; height: 301px;">
<pre><strong>Input:</strong> root = [1,2,3,4]
<strong>Output:</strong> "1(2(4))(3)"
<strong>Explanation:</strong> Originallay it needs to be "1(2(4)())(3()())", but you need to omit all the unnecessary empty parenthesis pairs. And it will be "1(2(4))(3)"
</pre>

<p><strong>Example 2:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/05/03/cons2-tree.jpg" style="width: 207px; height: 293px;">
<pre><strong>Input:</strong> root = [1,2,3,null,4]
<strong>Output:</strong> "1(2()(4))(3)"
Explanation: Almost the same as the first example, except we cannot omit the first parenthesis pair to break the one-to-one mapping relationship between the input and the output.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li>The number of nodes in the tree is in the range <code>[1, 10<sup>4</sup>]</code>.</li>
	<li><code>-1000 &lt;= Node.val &lt;= 1000</code></li>
</ul>


**Related Topics**:  
[String](https://leetcode.com/tag/string/), [Tree](https://leetcode.com/tag/tree/), [Depth-First Search](https://leetcode.com/tag/depth-first-search/), [Binary Tree](https://leetcode.com/tag/binary-tree/)

**Similar Questions**:
* [Construct Binary Tree from String (Medium)](https://leetcode.com/problems/construct-binary-tree-from-string/)
* [Find Duplicate Subtrees (Medium)](https://leetcode.com/problems/find-duplicate-subtrees/)

## Solution 1. DFS

```cpp
// OJ: https://leetcode.com/problems/construct-string-from-binary-tree/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    string ans;
    void dfs(TreeNode* root){
        if(!root)
            return;
        ans+= to_string(root->val);
        if(!root->left and !root->right)
            return;
        ans+="(";
        dfs(root->left);
        ans+=")";
        if(root->right){
            ans+="(";
            dfs(root->right);
            ans+=")";
        }
    }
    string tree2str(TreeNode* root) {
        dfs(root);
        return ans;
    }
};
```