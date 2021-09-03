# [297. Serialize and Deserialize Binary Tree (Hard)](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/)

<p>Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.</p>

<p>Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure.</p>

<p><strong>Clarification:</strong> The input/output format is the same as <a href="/faq/#binary-tree">how LeetCode serializes a binary tree</a>. You do not necessarily need to follow this format, so please be creative and come up with different approaches yourself.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2020/09/15/serdeser.jpg" style="width: 442px; height: 324px;">
<pre><strong>Input:</strong> root = [1,2,3,null,null,4,5]
<strong>Output:</strong> [1,2,3,null,null,4,5]
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> root = []
<strong>Output:</strong> []
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> root = [1]
<strong>Output:</strong> [1]
</pre>

<p><strong>Example 4:</strong></p>

<pre><strong>Input:</strong> root = [1,2]
<strong>Output:</strong> [1,2]
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li>The number of nodes in the tree is in the range <code>[0, 10<sup>4</sup>]</code>.</li>
	<li><code>-1000 &lt;= Node.val &lt;= 1000</code></li>
</ul>


**Related Topics**:  
[String](https://leetcode.com/tag/string/), [Tree](https://leetcode.com/tag/tree/), [Depth-First Search](https://leetcode.com/tag/depth-first-search/), [Breadth-First Search](https://leetcode.com/tag/breadth-first-search/), [Design](https://leetcode.com/tag/design/), [Binary Tree](https://leetcode.com/tag/binary-tree/)

**Similar Questions**:
* [Encode and Decode Strings (Medium)](https://leetcode.com/problems/encode-and-decode-strings/)
* [Serialize and Deserialize BST (Medium)](https://leetcode.com/problems/serialize-and-deserialize-bst/)
* [Find Duplicate Subtrees (Medium)](https://leetcode.com/problems/find-duplicate-subtrees/)
* [Serialize and Deserialize N-ary Tree (Hard)](https://leetcode.com/problems/serialize-and-deserialize-n-ary-tree/)

## Solution 1.

```cpp
// OJ: https://leetcode.com/problems/serialize-and-deserialize-binary-tree/
// Author: A M A N
// Time : O()
// Space: O()
class Codec {
class Codec {
public:
    //Encodes a tree to a single string
    string serialize(TreeNode* root){
        if(!root)
            return string("|");
        return to_string(root->val) + string("|") + serialize(root->left) + serialize(root->right);
    }
    TreeNode* buildTree(stringstream& ss){
        string token;
        getline(ss, token, '|');
        if(token == "")
            return nullptr;
        TreeNode* root = new TreeNode(stoi(token));
        root->left = buildTree(ss);
        root->right = buildTree(ss);
        return root;
    }
    //Decodes your encoded data to tree
    TreeNode* deserialize(string data){
        stringstream ss(data);
        return buildTree(ss);
    }
};
```