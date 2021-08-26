# [331. Verify Preorder Serialization of a Binary Tree (Medium)](https://leetcode.com/problems/verify-preorder-serialization-of-a-binary-tree/)

<p>One way to serialize a binary tree is to use <strong>preorder traversal</strong>. When we encounter a non-null node, we record the node's value. If it is a null node, we record using a sentinel value such as <code>'#'</code>.</p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/03/12/pre-tree.jpg" style="width: 362px; height: 293px;">
<p>For example, the above binary tree can be serialized to the string <code>"9,3,4,#,#,1,#,#,2,#,6,#,#"</code>, where <code>'#'</code> represents a null node.</p>

<p>Given a string of comma-separated values <code>preorder</code>, return <code>true</code> if it is a correct preorder traversal serialization of a binary tree.</p>

<p>It is <strong>guaranteed</strong> that each comma-separated value in the string must be either an integer or a character <code>'#'</code> representing null pointer.</p>

<p>You may assume that the input format is always valid.</p>

<ul>
	<li>For example, it could never contain two consecutive commas, such as <code>"1,,3"</code>.</li>
</ul>

<p><strong>Note:&nbsp;</strong>You are not allowed to reconstruct the tree.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<pre><strong>Input:</strong> preorder = "9,3,4,#,#,1,#,#,2,#,6,#,#"
<strong>Output:</strong> true
</pre><p><strong>Example 2:</strong></p>
<pre><strong>Input:</strong> preorder = "1,#"
<strong>Output:</strong> false
</pre><p><strong>Example 3:</strong></p>
<pre><strong>Input:</strong> preorder = "9,#,#,1"
<strong>Output:</strong> false
</pre>
<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= preorder.length &lt;= 10<sup>4</sup></code></li>
	<li><code>preoder</code> consist of integers in the range <code>[0, 100]</code> and <code>'#'</code> separated by commas <code>','</code>.</li>
</ul>


**Companies**:  
[Google](https://leetcode.com/company/google)

**Related Topics**:  
[String](https://leetcode.com/tag/string/), [Stack](https://leetcode.com/tag/stack/), [Tree](https://leetcode.com/tag/tree/), [Binary Tree](https://leetcode.com/tag/binary-tree/)

## Solution 1.

```cpp
// OJ: https://leetcode.com/problems/verify-preorder-serialization-of-a-binary-tree/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    vector<string> parseVals(string s){
        vector<string> res;
        string temp;
        for(int i=0; i<s.size(); i++){
            if(s[i]==','){
                res.push_back(temp);
                temp.clear();
            }else
                temp+=s[i];
        }
        res.push_back(temp); // the last string
        return res;
    }
    bool isValidSerialization(string preorder) {
        vector<string> vals = parseVals(preorder);
        int slots=1;
        for (auto s: vals){
            slots--;//one node takes one slot
            if(slots<0)return false;
            if(s!="#") slots+=2; // non null nodes can add two children
        }
        return slots==0;
    }
};
```

Shorter implementation.
```cpp
// OJ: https://leetcode.com/problems/verify-preorder-serialization-of-a-binary-tree/
// Author: A M A N
// Time : O(N)
// Space: O(1)
class Solution {
public:
    bool isValidSerialization(string preorder) {
        stringstream ss(preorder);
        string node;
        int slots=1;
        while(getline(ss, node, ',')){
            slots--; //one node takes one slot
            if(slots<0) return false;            
            if(node!="#") slots+=2; non null nodes can add two children
        }
        return slots==0;
    }
};
```