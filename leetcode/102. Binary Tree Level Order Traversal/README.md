# [102. Binary Tree Level Order Traversal (Medium)](https://leetcode.com/problems/binary-tree-level-order-traversal/)

<p>Given the <code>root</code> of a binary tree, return <em>the level order traversal of its nodes' values</em>. (i.e., from left to right, level by level).</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/02/19/tree1.jpg" style="width: 277px; height: 302px;">
<pre><strong>Input:</strong> root = [3,9,20,null,null,15,7]
<strong>Output:</strong> [[3],[9,20],[15,7]]
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
	<li><code>-1000 &lt;= Node.val &lt;= 1000</code></li>
</ul>


**Related Topics**:  
[Tree](https://leetcode.com/tag/tree/), [Breadth-First Search](https://leetcode.com/tag/breadth-first-search/), [Binary Tree](https://leetcode.com/tag/binary-tree/)

**Similar Questions**:
* [Binary Tree Zigzag Level Order Traversal (Medium)](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)
* [Binary Tree Level Order Traversal II (Medium)](https://leetcode.com/problems/binary-tree-level-order-traversal-ii/)
* [Minimum Depth of Binary Tree (Easy)](https://leetcode.com/problems/minimum-depth-of-binary-tree/)
* [Binary Tree Vertical Order Traversal (Medium)](https://leetcode.com/problems/binary-tree-vertical-order-traversal/)
* [Average of Levels in Binary Tree (Easy)](https://leetcode.com/problems/average-of-levels-in-binary-tree/)
* [N-ary Tree Level Order Traversal (Medium)](https://leetcode.com/problems/n-ary-tree-level-order-traversal/)
* [Cousins in Binary Tree (Easy)](https://leetcode.com/problems/cousins-in-binary-tree/)


## Solution 1. Inorder - using Map 

```cpp
// OJ: https://leetcode.com/problems/binary-tree-level-order-traversal/
// Author: A M A N
// Time : O(N * logN)
// Space: O(N)
class Solution {
public:
    map<int, vector<int>> mp;
    void dfs(TreeNode* u, int level){
        if(!u) return;
        mp[level].push_back(u->val);
        dfs(u->left, level+1);
        dfs(u->right, level+1);
    }
    vector<vector<int>> levelOrder(TreeNode* root) {
        dfs(root, 0);
        vector<vector<int>> ans;
        for(auto [a,v]: mp)
            ans.push_back(v);
        return ans;
    }
};

```
## Solution 2. Inorder - using no additional data structure

```cpp
// OJ: https://leetcode.com/problems/binary-tree-level-order-traversal/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    vector<vector<int>> ans;
    void dfs(TreeNode* u, int level){
        if(!u) return;
        if(ans.size()==level) // since we are not predefining the size of ans vec vec, when we encounter size limit
            // we increase the size
            ans.push_back(vector<int>());
        ans[level].push_back(u->val);
        dfs(u->left, level+1);
        dfs(u->right, level+1);
    }
    vector<vector<int>> levelOrder(TreeNode* root) {
        dfs(root, 0);
        return ans;
    }
};
```

## Solution 3. BFS - using queue

```cpp
// OJ: https://leetcode.com/problems/binary-tree-level-order-traversal/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> ans;
        queue<TreeNode *> q;
        if (root) 
            q.push(root);
        while (!q.empty()) {
            int len = q.size();
            vector<int> level;
            for (int i = 0;i < len;++i) {
                TreeNode *node = q.front();
                level.push_back(node->val);
                if (node->left) q.push(node->left);
                if (node->right) q.push(node->right);
                q.pop();
            }
            ans.push_back(level);
        }
    return ans;
}
};
```

## Solution 4. BFS - using stack and map
```cpp
// OJ: https://leetcode.com/problems/binary-tree-level-order-traversal/
// Author: A M A N
// Time : O(N * log N)
// Space: O(n)
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
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
        for(auto [a,v]:mp){
            //because we're using stack, FILO
            reverse(v.begin(), v.end());
            ans.push_back(v);
        }
        return ans;
    }
};
```

## Solution 5. BFS - using stack and vector

```cpp
// OJ: https://leetcode.com/problems/binary-tree-level-order-traversal/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> ans;
        map<int, vector<int>> mp;
        stack<pair<TreeNode*, int>> st;
        st.push({root, 0});
        while(st.size()){
            auto [u, level] = st.top(); st.pop();
            if(!u) continue;
            if(ans.size()==level) ans.push_back(vector<int>());
            ans[level].push_back(u->val);
            st.push({u->left, level+1});
            st.push({u->right, level+1});
        }
        for(auto &v:ans){
            //because we're using stack, FILO
            reverse(v.begin(), v.end());
        }
        return ans;
    }
};
```