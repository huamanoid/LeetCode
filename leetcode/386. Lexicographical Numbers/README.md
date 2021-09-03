# [386. Lexicographical Numbers (Medium)](https://leetcode.com/problems/lexicographical-numbers/)

<p>Given an integer <code>n</code>, return all the numbers in the range <code>[1, n]</code> sorted in lexicographical order.</p>

<p>You must write an algorithm that runs in&nbsp;<code>O(n)</code>&nbsp;time and uses <code>O(1)</code> extra space.&nbsp;</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<pre><strong>Input:</strong> n = 13
<strong>Output:</strong> [1,10,11,12,13,2,3,4,5,6,7,8,9]
</pre><p><strong>Example 2:</strong></p>
<pre><strong>Input:</strong> n = 2
<strong>Output:</strong> [1,2]
</pre>
<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 5 * 10<sup>4</sup></code></li>
</ul>


**Related Topics**:  
[Depth-First Search](https://leetcode.com/tag/depth-first-search/), [Trie](https://leetcode.com/tag/trie/)

## Solution 1. Trie

```cpp
// OJ: https://leetcode.com/problems/lexicographical-numbers/
// Author: A M A N
// Time : O(N)
// Space: O(1)
class Trie {
private:
    struct TrieNode {
        bool end;
        TrieNode* children[10] = {nullptr};
        TrieNode(){
            end = false;
        }
    };
    TrieNode* root;
public:
    Trie(){
        root = new TrieNode();
    }
    void insert(string word){
        auto node = root;
        for(char c: word){
            int idx = c-'0';
            if(node->children[idx]==nullptr)
                node->children[idx] = new TrieNode();
            node = node->children[idx];
        }
        node->end = true;
    }
    void dfs(TrieNode* root, int temp, vector<int>& ans){
        if(root->end)
            ans.push_back(temp);
        for(int i=0; i<10; i++)
            if(root->children[i])
                dfs(root->children[i], temp*10+i, ans);
    }
    vector<int> lex(){
        vector<int> ans;
        dfs(root, 0, ans);
        return ans;
    }
    // Deconstructor. freeing up dynamically allocated memory
    ~Trie() {
        Destroy(root);
    }
    void Destroy(TrieNode *node) {
        for(auto child: node->children) {
            if(child != nullptr)
                Destroy(child);
        }
        delete node;
    }
};
class Solution {
public:
    vector<int> lexicalOrder(int n) {
        Trie* t = new Trie();
        for(int i=1; i<=n; i++){
            string s = to_string(i);
            t->insert(s);
        }
        return t->lex();
    }
};
```

## Solution 2. DFS



```cpp
The idea is pretty simple. If we look at the order we can find out we just keep adding digit from 0 to 9 to every digit and make it a tree.
Then we visit every node in pre-order. 
       1        2        3    ...
      /\        /\       /\
   10 ...19  20...29  30...39   ....

// OJ: https://leetcode.com/problems/lexicographical-numbers/
// Author: A M A N
// Time : O(N)
// Space: O(1) 
class Solution {
public:
    void dfs(int cur, int n, vector<int>& ans){
        if(cur<=n){
            ans.push_back(cur);
            for(int i=0; i<=9; i++)
                dfs(cur*10+i, n, ans);
        }
    }
    vector<int> lexicalOrder(int n) {
        vector<int> ans;
        for(int i=1; i<=9; i++)
            dfs(i, n, ans);
        return ans;
    }
};
```