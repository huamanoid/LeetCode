# [95. Unique Binary Search Trees II (Medium)](https://leetcode.com/problems/unique-binary-search-trees-ii/)

<p>Given an integer <code>n</code>, return <em>all the structurally unique <strong>BST'</strong>s (binary search trees), which has exactly </em><code>n</code><em> nodes of unique values from</em> <code>1</code> <em>to</em> <code>n</code>. Return the answer in <strong>any order</strong>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/01/18/uniquebstn3.jpg" style="width: 600px; height: 148px;">
<pre><strong>Input:</strong> n = 3
<strong>Output:</strong> [[1,null,2,null,3],[1,null,3,2],[2,1,3],[3,1,null,null,2],[3,2,null,1]]
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> n = 1
<strong>Output:</strong> [[1]]
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 8</code></li>
</ul>


**Related Topics**:  
[Dynamic Programming](https://leetcode.com/tag/dynamic-programming/), [Backtracking](https://leetcode.com/tag/backtracking/), [Tree](https://leetcode.com/tag/tree/), [Binary Search Tree](https://leetcode.com/tag/binary-search-tree/), [Binary Tree](https://leetcode.com/tag/binary-tree/)

**Similar Questions**:
* [Unique Binary Search Trees (Medium)](https://leetcode.com/problems/unique-binary-search-trees/)
* [Different Ways to Add Parentheses (Medium)](https://leetcode.com/problems/different-ways-to-add-parentheses/)

## Solution 1.

```cpp
// OJ: https://leetcode.com/problems/unique-binary-search-trees-ii/
// Author: A M A N
// Time : O(N * C(N)) or (4^N)
// Space: O()
//where C(N) is catalan number = (4^n)/((n^(3/2))*sqrt(pi))
};
class Solution {
public:
    vector<TreeNode*> buildBST(int i, int j){
        vector<TreeNode*> ans;
        //Base Case
        if(i>j) {
            ans.push_back(NULL);
            return ans;
        }
        //since this problem seemed to be solvable by resursion
        //we divide the structure into left and right
        //by using a variable k
        for(int k=i; k<=j; k++){
            //Hypothesis
            auto l = buildBST(i, k-1);            
            auto r = buildBST(k+1, j);
            //Induction
            for(auto leftTree: l){
                for(auto rightTree: r){
                    TreeNode* rootNode = new TreeNode(k);
                    rootNode->left  =  leftTree;
                    rootNode->right =  rightTree;
                    ans.push_back(rootNode);
                }
            }
        }
        return ans;
    }
    vector<TreeNode*> generateTrees(int n) {
        return buildBST(1, n);
    }
};
```