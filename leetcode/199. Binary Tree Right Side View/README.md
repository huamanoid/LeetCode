# [199. Binary Tree Right Side View (Medium)](https://leetcode.com/problems/binary-tree-right-side-view/)

<p>Given the <code>root</code> of a binary tree, imagine yourself standing on the <strong>right side</strong> of it, return <em>the values of the nodes you can see ordered from top to bottom</em>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/02/14/tree.jpg" style="width: 401px; height: 301px;">
<pre><strong>Input:</strong> root = [1,2,3,null,5,null,4]
<strong>Output:</strong> [1,3,4]
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> root = [1,null,3]
<strong>Output:</strong> [1,3]
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> root = []
<strong>Output:</strong> []
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li>The number of nodes in the tree is in the range <code>[0, 100]</code>.</li>
	<li><code>-100 &lt;= Node.val &lt;= 100</code></li>
</ul>


**Related Topics**:  
[Tree](https://leetcode.com/tag/tree/), [Depth-First Search](https://leetcode.com/tag/depth-first-search/), [Breadth-First Search](https://leetcode.com/tag/breadth-first-search/), [Binary Tree](https://leetcode.com/tag/binary-tree/)

**Similar Questions**:
* [Populating Next Right Pointers in Each Node (Medium)](https://leetcode.com/problems/populating-next-right-pointers-in-each-node/)
* [Boundary of Binary Tree (Medium)](https://leetcode.com/problems/boundary-of-binary-tree/)

## Solution 1. DFS

```cpp
// OJ: https://leetcode.com/problems/binary-tree-right-side-view/
// Author: A M A N
// Time : O(N)
// Space: O(N)
};
class Solution {
public:
    void recursion(TreeNode *root, int level, vector<int> &res){
        if(!root) 
            return ;
//         Charm here or we can just use a map from level to 
        if(res.size()<level) 
            res.push_back(root->val);
//      ***visiting right subtree first***
        recursion(root->right, level+1, res);
        recursion(root->left, level+1, res);
    }
    vector<int> rightSideView(TreeNode *root) {
        vector<int> res;
        recursion(root, 1, res);
        return res;
    }
};
```


## Solution 2. BFS

```cpp
// OJ: https://leetcode.com/problems/binary-tree-right-side-view/
// Author: A M A N
// Time : O(N)
// Space: O(N)
};
class Solution {
public:
    BFS
    vector<int> rightSideView(TreeNode* root) {
        map<int, int>mp;      
        queue<pair<TreeNode*, int>> q;
        q.push({root, 0});
        while(q.size()){
            auto [cur, level] = q.front(); q.pop();
            if(!cur)continue;
            mp[level] = (cur->val);
            q.push({cur->left, level+1});
            q.push({cur->right, level+1});
        }
        vector<int> ans;
        for(auto [a,vec]: mp)
            ans.push_back(vec);
        return ans;
    }
};
```

