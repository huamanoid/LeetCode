# [938. Range Sum of BST (Easy)](https://leetcode.com/problems/range-sum-of-bst/)

<p>Given the <code>root</code> node of a binary search tree and two integers <code>low</code> and <code>high</code>, return <em>the sum of values of all nodes with a value in the <strong>inclusive</strong> range </em><code>[low, high]</code>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2020/11/05/bst1.jpg" style="width: 400px; height: 222px;">
<pre><strong>Input:</strong> root = [10,5,15,3,7,null,18], low = 7, high = 15
<strong>Output:</strong> 32
<strong>Explanation:</strong> Nodes 7, 10, and 15 are in the range [7, 15]. 7 + 10 + 15 = 32.
</pre>

<p><strong>Example 2:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2020/11/05/bst2.jpg" style="width: 400px; height: 335px;">
<pre><strong>Input:</strong> root = [10,5,15,3,7,13,18,1,null,6], low = 6, high = 10
<strong>Output:</strong> 23
<strong>Explanation:</strong> Nodes 6, 7, and 10 are in the range [6, 10]. 6 + 7 + 10 = 23.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li>The number of nodes in the tree is in the range <code>[1, 2 * 10<sup>4</sup>]</code>.</li>
	<li><code>1 &lt;= Node.val &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= low &lt;= high &lt;= 10<sup>5</sup></code></li>
	<li>All <code>Node.val</code> are <strong>unique</strong>.</li>
</ul>


**Related Topics**:  
[Tree](https://leetcode.com/tag/tree/), [Depth-First Search](https://leetcode.com/tag/depth-first-search/), [Binary Search Tree](https://leetcode.com/tag/binary-search-tree/), [Binary Tree](https://leetcode.com/tag/binary-tree/)

## Solution 1. Recursion (Un-optimized)
Visiting all nodes and accumulating if it's in the range

```cpp
// OJ: https://leetcode.com/problems/range-sum-of-bst/
// Author: A M A N
// Time: O(N)
// Space: O(N)
class Solution {
public:
    int rangeSumBST(TreeNode* root, int low, int high) {
        if(!root) return 0;
        int val = (root->val>=low and root->val<=high ? root->val:0);
        return rangeSumBST(root->left, low, high) + rangeSumBST(root->right, low, high) + val;
    }
};
```

## Solution 2. Recursion (Optimized)
Using the sorted property of BST to traverse only the viable nodes
```cpp
// OJ: https://leetcode.com/problems/range-sum-of-bst/
// Author: A M A N
// Time: O(N)
// Space: O(N)
class Solution {
public:
    int ans;
    int rangeSumBST(TreeNode* root, int low, int high) {
        if(!root)
            return 0;
        if(root->val >= low and root->val <=high)
            ans+= root->val;
        if(low < root->val)
            rangeSumBST(root->left, low, high);
        if(high > root->val)
            rangeSumBST(root->right, low, high);
        return ans;
    }
};
```

## Solution 3. BFS

```cpp
// OJ: https://leetcode.com/problems/range-sum-of-bst/
// Author: A M A N
// Time: O(N)
// Space: O(N)
class Solution {
public:
    int rangeSumBST(TreeNode* root, int low, int high) {
        if(!root)
            return 0;
        int ans = 0;
        stack<TreeNode*> st;
        st.push(root);
        while(st.size()){
            TreeNode* cur = st.top();  st.pop();
            int val = cur->val;
            if(val>= low and val<=high)
                ans+= val;
            if(low<val and cur->left)
                st.push(cur->left);
            if(high>val and cur->right)
                st.push(cur->right);
        }
        return ans;
    }
};
```
