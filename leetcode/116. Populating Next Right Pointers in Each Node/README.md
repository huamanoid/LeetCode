# [116. Populating Next Right Pointers in Each Node (Medium)](https://leetcode.com/problems/populating-next-right-pointers-in-each-node/)

<p>You are given a <strong>perfect binary tree</strong>&nbsp;where&nbsp;all leaves are on the same level, and every parent has two children. The binary tree has the following definition:</p>

<pre>struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
</pre>

<p>Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to <code>NULL</code>.</p>

<p>Initially, all next pointers are set to <code>NULL</code>.</p>

<p>&nbsp;</p>

<p><strong>Follow up:</strong></p>

<ul>
	<li>You may only use constant extra space.</li>
	<li>Recursive approach is fine, you may assume implicit stack space does not count as extra space for this problem.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<p><img alt="" src="https://assets.leetcode.com/uploads/2019/02/14/116_sample.png" style="width: 640px; height: 218px;"></p>

<pre><strong>Input:</strong> root = [1,2,3,4,5,6,7]
<strong>Output:</strong> [1,#,2,3,#,4,5,6,7,#]
<strong>Explanation: </strong>Given the above perfect binary tree (Figure A), your function should populate each next pointer to point to its next right node, just like in Figure B. The serialized output is in level order as connected by the next pointers, with '#' signifying the end of each level.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li>The number of nodes in the given tree is less than <code>4096</code>.</li>
	<li><code>-1000 &lt;= node.val &lt;= 1000</code></li>
</ul>

**Related Topics**:  
[Tree](https://leetcode.com/tag/tree/), [Depth-First Search](https://leetcode.com/tag/depth-first-search/), [Breadth-First Search](https://leetcode.com/tag/breadth-first-search/), [Binary Tree](https://leetcode.com/tag/binary-tree/)

**Similar Questions**:
* [Populating Next Right Pointers in Each Node II (Medium)](https://leetcode.com/problems/populating-next-right-pointers-in-each-node-ii/)
* [Binary Tree Right Side View (Medium)](https://leetcode.com/problems/binary-tree-right-side-view/)

## Solution 1. DFS - Level order traversal

```cpp
// OJ: https://leetcode.com/problems/populating-next-right-pointers-in-each-node/
// Author: A M A N
// Time : O(N)
// Space: O(N)
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* left;
    Node* right;
    Node* next;
    Node() : val(0), left(NULL), right(NULL), next(NULL) {}
    Node(int _val) : val(_val), left(NULL), right(NULL), next(NULL) {}
    Node(int _val, Node* _left, Node* _right, Node* _next)
        : val(_val), left(_left), right(_right), next(_next) {}
};
*/
class Solution {
public:
    vector<vector<Node*>> v;
    void dfs(Node* root, int level){
        if(!root)
            return;
        if(level == v.size())
            v.push_back(vector<Node*>());
        v[level].push_back(root);
        dfs(root->left, level+1);
        dfs(root->right, level+1);
    }
    Node* connect(Node* root) {
        dfs(root, 0);
        for(int i=0; i<v.size(); i++){
            for(int j=0; j+1<v[i].size(); j++){
                v[i][j]->next = v[i][j+1];
            }
            v[i][v[i].size()-1]->next = nullptr;
        }
        return root;
    }
};
```


## Solution 2. Iterative - Level order traversal

```cpp
// OJ: https://leetcode.com/problems/populating-next-right-pointers-in-each-node/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    Node* connect(Node* root) {
        queue<Node*> q;
        q.push(root);
        while(q.size()){
            int n = q.size();
            for(int i=0; i<n; i++){
                auto u = q.front(); q.pop();
                if(!u)continue;
                if(i==n-1)
                    u->next = nullptr;
                else
                    u->next = q.front();
                q.push(u->left);
                q.push(u->right);
            }
        }
        return root;
    }
};
```

## Solution 3. Recursion

```cpp
// OJ: https://leetcode.com/problems/populating-next-right-pointers-in-each-node/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    Node* connect(Node* root) {
        if(!root)
            return root;
        if(root->left) root->left->next = root->right;
        if(root->right and root->next) root->right->next = root->next->left;
        connect(root->left);
        connect(root->right);
        return root;
    }
};
```

## Solution 4. Iterative (*) 

```cpp
// OJ: https://leetcode.com/problems/populating-next-right-pointers-in-each-node/
// Author: A M A N
// Time : O(N)
// Space: O(1)
class Solution {
public:
    Node* connect(Node* root) {
        if(!root)
            return root;
        Node* level_start = root;
        while(level_start){
            Node* cur = level_start;
            while(cur){
                if(cur->left) cur->left->next = cur->right;
                if(cur->right and cur->next) cur->right->next = cur->next->left;
                cur = cur->next;
            }
            level_start = level_start->left;
        }
        return root;
    }
};
```