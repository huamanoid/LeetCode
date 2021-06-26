# [889. Construct Binary Tree from Preorder and Postorder Traversal (Medium)](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-postorder-traversal/)

<p>Return any binary tree that matches the given preorder and postorder traversals.</p>

<p>Values in the traversals&nbsp;<code>pre</code> and <code>post</code>&nbsp;are distinct&nbsp;positive integers.</p>

<p>&nbsp;</p>

<div>
<p><strong>Example 1:</strong></p>

<pre><strong>Input: </strong>pre = <span id="example-input-1-1">[1,2,4,5,3,6,7]</span>, post = <span id="example-input-1-2">[4,5,2,6,7,3,1]</span>
<strong>Output: </strong><span id="example-output-1">[1,2,3,4,5,6,7]</span>
</pre>

<p>&nbsp;</p>

<p><strong><span>Note:</span></strong></p>

<ul>
	<li><code>1 &lt;= pre.length == post.length &lt;= 30</code></li>
	<li><code>pre[]</code> and <code>post[]</code>&nbsp;are both permutations of <code>1, 2, ..., pre.length</code>.</li>
	<li>It is guaranteed an answer exists. If there exists multiple answers, you can return any of them.</li>
</ul>
</div>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Hash Table](https://leetcode.com/tag/hash-table/), [Divide and Conquer](https://leetcode.com/tag/divide-and-conquer/), [Tree](https://leetcode.com/tag/tree/), [Binary Tree](https://leetcode.com/tag/binary-tree/)

## Solution 1. Divide & Conquer

```cpp
// OJ: https://leetcode.com/problems/construct-binary-tree-from-preorder-and-postorder-traversal/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    unordered_map<int, int> postIdx;
    TreeNode* solve(vector<int>&pre, int preStart, int preEnd, vector<int>&post, int postStart, int postEnd){
        if(preStart>preEnd)
            return nullptr;
        TreeNode* t = new TreeNode(pre[preStart]);
        if(preStart==preEnd)
            return t;
        int idx = postIdx[pre[preStart+1]];
        int sizeOfLeft = idx - postStart; //post order is helping us give the size of the left and right subtree
        //        (*)              
        // [root][----left----][----right----]
        //            (*)
        // [----left----][----right----][root]
        t->left  = solve(pre, preStart+1,              preStart+1+sizeOfLeft, post, postStart, idx); 
        t->right = solve(pre, preStart+1+sizeOfLeft+1, preEnd,                post, idx+1,     postEnd);
        return t;
    }
    TreeNode* constructFromPrePost(vector<int>& pre, vector<int>& post) {
        int n = post.size();
        for(int i=0; i<post.size(); i++)
            postIdx[post[i]] = i;
        return solve(pre, 0, n-1, post, 0, n-1);
    }
};
```