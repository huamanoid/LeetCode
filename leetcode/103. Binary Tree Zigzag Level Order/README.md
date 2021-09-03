# [103. Binary Tree Zigzag Level Order Traversal (Medium)](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)

<p>Given the <code>root</code> of a binary tree, return <em>the zigzag level order traversal of its nodes' values</em>. (i.e., from left to right, then right to left for the next level and alternate between).</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/02/19/tree1.jpg" style="width: 277px; height: 302px;">
<pre><strong>Input:</strong> root = [3,9,20,null,null,15,7]
<strong>Output:</strong> [[3],[20,9],[15,7]]
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> root = [1]
<strong>Output:</strong> [[1]]
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> root = []
<strong>Output:</strong> []
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li>The number of nodes in the tree is in the range <code>[0, 2000]</code>.</li>
	<li><code>-100 &lt;= Node.val &lt;= 100</code></li>
</ul>


**Related Topics**:  
[Tree](https://leetcode.com/tag/tree/), [Breadth-First Search](https://leetcode.com/tag/breadth-first-search/), [Binary Tree](https://leetcode.com/tag/binary-tree/)

**Similar Questions**:
* [Binary Tree Level Order Traversal (Medium)](https://leetcode.com/problems/binary-tree-level-order-traversal/)


## Solution 1. DFS

```cpp
// OJ: https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    map<int, vector<int>> mp;
    void dfs(TreeNode* u, int level){
        if(!u)
            return;
        mp[level].push_back(u->val);
        dfs(u->left, level+1);
        dfs(u->right, level+1);
    }
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        dfs(root, 0);
        vector<vector<int>> ans;
        bool rev = 1;
        for(auto [a,v]: mp){
            if(rev^=1)
                reverse(v.begin(), v.end());
            ans.push_back(v);
        }
        return ans;
    }
};
```


## Solution 2. BFS (Level order traversal)

```cpp
// OJ: https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        vector<vector<int>> ans;
        map<int, vector<int>> mp;
        stack<pair<TreeNode*, int>> st;
        st.push({root, 0});
        while(st.size()){
            auto [u, level] = st.top(); st.pop();
            if(!u) continue;
            mp[level].push_back(u->val);
            st.push({u->left, level+1});
            st.push({u->right, level+1});
        }
        bool rev = 0;
        for(auto [a,v]:mp){
            if(rev^=1)
                reverse(v.begin(), v.end());
            ans.push_back(v);
        }
        return ans;
    }
};
```


