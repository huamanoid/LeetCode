# [662. Maximum Width of Binary Tree (Medium)](https://leetcode.com/problems/maximum-width-of-binary-tree/)

<p>Given the <code>root</code> of a binary tree, return <em>the <strong>maximum width</strong> of the given tree</em>.</p>

<p>The <strong>maximum width</strong> of a tree is the maximum <strong>width</strong> among all levels.</p>

<p>The <strong>width</strong> of one level is defined as the length between the end-nodes (the leftmost and rightmost non-null nodes), where the null nodes between the end-nodes are also counted into the length calculation.</p>

<p>It is <strong>guaranteed</strong> that the answer will in the range of <strong>32-bit</strong> signed integer.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/05/03/width1-tree.jpg" style="width: 359px; height: 302px;">
<pre><strong>Input:</strong> root = [1,3,2,5,3,null,9]
<strong>Output:</strong> 4
<strong>Explanation:</strong> The maximum width existing in the third level with the length 4 (5,3,null,9).
</pre>

<p><strong>Example 2:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/05/03/width2-tree.jpg" style="width: 224px; height: 302px;">
<pre><strong>Input:</strong> root = [1,3,null,5,3]
<strong>Output:</strong> 2
<strong>Explanation:</strong> The maximum width existing in the third level with the length 2 (5,3).
</pre>

<p><strong>Example 3:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/05/03/width3-tree.jpg" style="width: 289px; height: 299px;">
<pre><strong>Input:</strong> root = [1,3,2,5]
<strong>Output:</strong> 2
<strong>Explanation:</strong> The maximum width existing in the second level with the length 2 (3,2).
</pre>

<p><strong>Example 4:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/05/03/width4-tree.jpg" style="width: 500px; height: 244px;">
<pre><strong>Input:</strong> root = [1,3,2,5,null,null,9,6,null,null,7]
<strong>Output:</strong> 8
<strong>Explanation:</strong> The maximum width existing in the fourth level with the length 8 (6,null,null,null,null,null,null,7).
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li>The number of nodes in the tree is in the range <code>[1, 3000]</code>.</li>
	<li><code>-100 &lt;= Node.val &lt;= 100</code></li>
</ul>


**Related Topics**:  
[Tree](https://leetcode.com/tag/tree/), [Depth-First Search](https://leetcode.com/tag/depth-first-search/), [Breadth-First Search](https://leetcode.com/tag/breadth-first-search/), [Binary Tree](https://leetcode.com/tag/binary-tree/)

## Solution 1. DFS 
### calculating maximum difference from leftmost nodes on that level and using indexing of nodes 
```
// [idx], left child => [2*idx], right child => [2*idx + 1] 
              [1]
            /     \
         /          \
      [2]             [3]
     /   \          /      \
   [4]    [5]     [6]       [7]
  /  \    /  \    /   \     /   \
[8] [9] [10] [11] [12] [13] [14] [15]    
// index range of a level is basically no of numbers whose level(th) bit is set. i.e. for 3rd level => (1000 to 1111)  which is 8 to 15
// notice the similarity of binary trees with the bit representation of numbers.
```
```cpp
// OJ: https://leetcode.com/problems/maximum-width-of-binary-tree/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
// replace int with unsigned long long to handle overflow
// since the indices grows exponentially, and can cause overflow for slim and tall trees

    int dfs(TreeNode* root, int idx, int level, vector<int> &left){ //passing left by reference otherwise for each subtree there would be a left vector seperately which wont have the left most node inserted and it will insert again the leftmost node of current subtree
        if(!root)
            return 0;
        if(level >= left.size()) //reached to a depper level
            left.push_back(idx); //adds left most node
        int l = dfs(root->left, 2*idx, level+1, left);
        int r = dfs(root->right, 2*idx+1, level+1, left);
        return max({idx - left[level] + 1, l, r});
    }
    int widthOfBinaryTree(TreeNode* root) {
        vector<int> left;
        return dfs(root, 1, 0, left);
    }
};

```


## BFS 

```cpp
// OJ: https://leetcode.com/problems/maximum-width-of-binary-tree/
// Author: A M A N
// Time : O(N)
// Space: O(log N)
class Solution {
public:
    // BFS
    int widthOfBinaryTree(TreeNode* root){
        if (!root) return 0;
        unsigned long long ans = 0;
        queue<pair<TreeNode*, unsigned long long>> q;
        q.push({root, 1});// node and its index
        while(!q.empty()){
            unsigned long long l = q.front().second, r = l;
            unsigned long long n = q.size(); //important step, can't write i<q.size() in the for loop, as we're popping it
            for(int i=0; i<n; i++){
                TreeNode* u = q.front().first;
                r = q.front().second;
                q.pop();
                if(u->left)  q.push({u->left,  2*r});
                if(u->right) q.push({u->right, 2*r+1});
            }
            ans = max(r-l+1, ans);
        }
        return ans;
    }
};
```