# [437. Path Sum III (Medium)](https://leetcode.com/problems/path-sum-iii/)

<p>Given the <code>root</code> of a binary tree and an integer <code>targetSum</code>, return <em>the number of paths where the sum of the values&nbsp;along the path equals</em>&nbsp;<code>targetSum</code>.</p>

<p>The path does not need to start or end at the root or a leaf, but it must go downwards (i.e., traveling only from parent nodes to child nodes).</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/04/09/pathsum3-1-tree.jpg" style="width: 450px; height: 386px;">
<pre><strong>Input:</strong> root = [10,5,-3,3,2,null,11,3,-2,null,1], targetSum = 8
<strong>Output:</strong> 3
<strong>Explanation:</strong> The paths that sum to 8 are shown.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22
<strong>Output:</strong> 3
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li>The number of nodes in the tree is in the range <code>[0, 1000]</code>.</li>
	<li><code>-10<sup>9</sup> &lt;= Node.val &lt;= 10<sup>9</sup></code></li>
	<li><code>-1000 &lt;= targetSum &lt;= 1000</code></li>
</ul>


**Related Topics**:  
[Tree](https://leetcode.com/tag/tree/), [Depth-First Search](https://leetcode.com/tag/depth-first-search/), [Binary Tree](https://leetcode.com/tag/binary-tree/)

**Similar Questions**:
* [Path Sum (Easy)](https://leetcode.com/problems/path-sum/)
* [Path Sum II (Medium)](https://leetcode.com/problems/path-sum-ii/)
* [Path Sum IV (Medium)](https://leetcode.com/problems/path-sum-iv/)
* [Longest Univalue Path (Medium)](https://leetcode.com/problems/longest-univalue-path/)

## Solution 1. DFS + Prefix sum 

```cpp
// OJ: https://leetcode.com/problems/path-sum-iii/
// Author: A M A N
// Time : O()
// Space: O()
class Solution {
public:
    int ans;
    void solve(TreeNode* root, int targetSum, int curSum, unordered_map<int, int> preSum){
        if(!root)
            return;
        curSum+= root->val;
        if(preSum.find(curSum-targetSum)!=preSum.end())
            ans+= preSum[curSum-targetSum];
        preSum[curSum]+=1;
        solve(root->left, targetSum, curSum , preSum);
        solve(root->right, targetSum, curSum, preSum);
        preSum[curSum]-=1;
    }
    int pathSum(TreeNode* root, int targetSum) {
        unordered_map<int, int>preSum; //key: sum , val: no. of ways to get the sum
        preSum[0]=1; 
        solve(root, targetSum, 0, preSum);
        return ans;
    }
};
```