# [337. House Robber III (Medium)](https://leetcode.com/problems/house-robber-iii/)

<p>The thief has found himself a new place for his thievery again. There is only one entrance to this area, called <code>root</code>.</p>

<p>Besides the <code>root</code>, each house has one and only one parent house. After a tour, the smart thief realized that all houses in this place form a binary tree. It will automatically contact the police if <strong>two directly-linked houses were broken into on the same night</strong>.</p>

<p>Given the <code>root</code> of the binary tree, return <em>the maximum amount of money the thief can rob <strong>without alerting the police</strong></em>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/03/10/rob1-tree.jpg" style="width: 277px; height: 293px;">
<pre><strong>Input:</strong> root = [3,2,3,null,3,null,1]
<strong>Output:</strong> 7
<strong>Explanation:</strong> Maximum amount of money the thief can rob = 3 + 3 + 1 = 7.
</pre>

<p><strong>Example 2:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/03/10/rob2-tree.jpg" style="width: 357px; height: 293px;">
<pre><strong>Input:</strong> root = [3,4,5,1,3,null,1]
<strong>Output:</strong> 9
<strong>Explanation:</strong> Maximum amount of money the thief can rob = 4 + 5 = 9.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li>The number of nodes in the tree is in the range <code>[1, 10<sup>4</sup>]</code>.</li>
	<li><code>0 &lt;= Node.val &lt;= 10<sup>4</sup></code></li>
</ul>


**Related Topics**:  
[Dynamic Programming](https://leetcode.com/tag/dynamic-programming/), [Tree](https://leetcode.com/tag/tree/), [Depth-First Search](https://leetcode.com/tag/depth-first-search/), [Binary Tree](https://leetcode.com/tag/binary-tree/)

**Similar Questions**:
* [House Robber (Medium)](https://leetcode.com/problems/house-robber/)
* [House Robber II (Medium)](https://leetcode.com/problems/house-robber-ii/)

## Solution 1. DFS - DP

```cpp
// OJ: https://leetcode.com/problems/house-robber-iii/
// Author: A M A N
// Time : O()
// Space: O()
class Solution {
public:
    int ans;
    map<pair<TreeNode*, bool>, int> mp;
    int dfs(TreeNode* root, bool include){
        //Base Case
        if(!root)
            return 0;
        if(mp.find({root, include})!=mp.end())
            return mp[{root, include}];
        int l, r, lx, rx;
        //Hypothesis
        if(include){
            lx = dfs(root->left,  0);
            rx = dfs(root->right, 0);
            ans = max(ans, lx+rx+root->val);
            return lx + rx + root->val;
        }else{
            lx = dfs(root->left, 0);
            rx = dfs(root->right, 0);
            l = dfs(root->left,  1);
            r = dfs(root->right, 1);
            int temp = max(lx, l) + max(rx, r);
            ans = max({ans, temp});
            return mp[{root, include}] = temp;
        }
    }
    int rob(TreeNode* root) {
        dfs(root, 0);
        dfs(root, 1);
        return ans;
    }
};
```