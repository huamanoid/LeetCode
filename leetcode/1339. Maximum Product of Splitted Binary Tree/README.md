# [1339. Maximum Product of Splitted Binary Tree (Medium)](https://leetcode.com/problems/maximum-product-of-splitted-binary-tree/)

<p>Given the <code>root</code> of a binary tree, split the binary tree into two subtrees by removing one edge such that the product of the sums of the subtrees is maximized.</p>

<p>Return <em>the maximum product of the sums of the two subtrees</em>. Since the answer may be too large, return it <strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p><strong>Note</strong> that you need to maximize the answer before taking the mod and not after taking it.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2020/01/21/sample_1_1699.png" style="width: 500px; height: 167px;">
<pre><strong>Input:</strong> root = [1,2,3,4,5,6]
<strong>Output:</strong> 110
<strong>Explanation:</strong> Remove the red edge and get 2 binary trees with sum 11 and 10. Their product is 110 (11*10)
</pre>

<p><strong>Example 2:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2020/01/21/sample_2_1699.png" style="width: 500px; height: 211px;">
<pre><strong>Input:</strong> root = [1,null,2,3,4,null,null,5,6]
<strong>Output:</strong> 90
<strong>Explanation:</strong> Remove the red edge and get 2 binary trees with sum 15 and 6.Their product is 90 (15*6)
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> root = [2,3,9,10,7,8,6,5,4,11,1]
<strong>Output:</strong> 1025
</pre>

<p><strong>Example 4:</strong></p>

<pre><strong>Input:</strong> root = [1,1]
<strong>Output:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li>The number of nodes in the tree is in the range <code>[2, 5 * 10<sup>4</sup>]</code>.</li>
	<li><code>1 &lt;= Node.val &lt;= 10<sup>4</sup></code></li>
</ul>


**Companies**:  
[ByteDance](https://leetcode.com/company/bytedance), [Amazon](https://leetcode.com/company/amazon), [Microsoft](https://leetcode.com/company/microsoft)

**Related Topics**:  
[Tree](https://leetcode.com/tag/tree/), [Depth-First Search](https://leetcode.com/tag/depth-first-search/), [Binary Tree](https://leetcode.com/tag/binary-tree/)

## Solution 1. DFS

```cpp
// OJ: https://leetcode.com/problems/maximum-product-of-splitted-binary-tree/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    int64_t Sum=0;
    int64_t maxProd=1;
    const int M = 1e9+7;
    int64_t dfs(TreeNode* root){
        if(!root)
            return 0;
        int64_t l = dfs(root->left);
        int64_t r = dfs(root->right);
        maxProd = max(maxProd, max((Sum-l)*l, (Sum-r)*r));
        return root->val + l + r;
    }
    int maxProduct(TreeNode* root) {
        Sum = dfs(root);    
        dfs(root);
        return maxProd%M;
    }
};
```
solving with a single pass

```cpp
// OJ: https://leetcode.com/problems/maximum-product-of-splitted-binary-tree/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    const int M = 1e9+7;
    vector<int> sum;
    int dfs(TreeNode* root){
        if(!root)
            return 0;
        int l = dfs(root->left);
        int r = dfs(root->right);
        int s = root->val + l + r;
        sum.push_back(s);
        return s;
    }
    int maxProduct(TreeNode* root) {
        int64_t totalSum = dfs(root);
        int64_t maxProd=1;
        for(auto s: sum)
            maxProd = max(maxProd, s*(totalSum-s));
        return maxProd%M;
    }
};
```