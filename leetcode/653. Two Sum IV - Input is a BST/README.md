# [653. Two Sum IV - Input is a BST (Easy)](https://leetcode.com/problems/two-sum-iv-input-is-a-bst/)

<p>Given the <code>root</code> of a Binary Search Tree and a target number <code>k</code>, return <em><code>true</code> if there exist two elements in the BST such that their sum is equal to the given target</em>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2020/09/21/sum_tree_1.jpg" style="width: 562px; height: 322px;">
<pre><strong>Input:</strong> root = [5,3,6,2,4,null,7], k = 9
<strong>Output:</strong> true
</pre>

<p><strong>Example 2:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2020/09/21/sum_tree_2.jpg" style="width: 562px; height: 322px;">
<pre><strong>Input:</strong> root = [5,3,6,2,4,null,7], k = 28
<strong>Output:</strong> false
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> root = [2,1,3], k = 4
<strong>Output:</strong> true
</pre>

<p><strong>Example 4:</strong></p>

<pre><strong>Input:</strong> root = [2,1,3], k = 1
<strong>Output:</strong> false
</pre>

<p><strong>Example 5:</strong></p>

<pre><strong>Input:</strong> root = [2,1,3], k = 3
<strong>Output:</strong> true
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li>The number of nodes in the tree is in the range <code>[1, 10<sup>4</sup>]</code>.</li>
	<li><code>-10<sup>4</sup>&nbsp;&lt;= Node.val &lt;= 10<sup>4</sup></code></li>
	<li><code>root</code> is guaranteed to be a <strong>valid</strong> binary search tree.</li>
	<li><code>-10<sup>5</sup>&nbsp;&lt;= k &lt;= 10<sup>5</sup></code></li>
</ul>


**Related Topics**:  
[Hash Table](https://leetcode.com/tag/hash-table/), [Two Pointers](https://leetcode.com/tag/two-pointers/), [Tree](https://leetcode.com/tag/tree/), [Depth-First Search](https://leetcode.com/tag/depth-first-search/), [Breadth-First Search](https://leetcode.com/tag/breadth-first-search/), [Binary Search Tree](https://leetcode.com/tag/binary-search-tree/), [Binary Tree](https://leetcode.com/tag/binary-tree/)

**Similar Questions**:
* [Two Sum (Easy)](https://leetcode.com/problems/two-sum/)
* [Two Sum II - Input array is sorted (Easy)](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)
* [Two Sum III - Data structure design (Easy)](https://leetcode.com/problems/two-sum-iii-data-structure-design/)
* [Two Sum BSTs (Medium)](https://leetcode.com/problems/two-sum-bsts/)

## Solution 1. DFS

```cpp
// OJ: https://leetcode.com/problems/two-sum-iv-input-is-a-bst/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    unordered_map<int, int> mp;
    void dfs(TreeNode* root, int k, bool& found){
        if(!root)
            return;
        if(mp.find(k-root->val)!=mp.end())
            found = true;
        mp[root->val]++;
        dfs(root->left, k, found);
        dfs(root->right, k, found);
    }
    bool findTarget(TreeNode* root, int k) {
        bool found = false;
        dfs(root, k, found);
        return found;
    }
};
```