# [212. Word Search II (Hard)](https://leetcode.com/problems/word-search-ii/)

<p>Given an <code>m x n</code> <code>board</code>&nbsp;of characters and a list of strings <code>words</code>, return <em>all words on the board</em>.</p>

<p>Each word must be constructed from letters of sequentially adjacent cells, where <strong>adjacent cells</strong> are horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2020/11/07/search1.jpg" style="width: 322px; height: 322px;">
<pre><strong>Input:</strong> board = [["o","a","a","n"],["e","t","a","e"],["i","h","k","r"],["i","f","l","v"]], words = ["oath","pea","eat","rain"]
<strong>Output:</strong> ["eat","oath"]
</pre>

<p><strong>Example 2:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2020/11/07/search2.jpg" style="width: 162px; height: 162px;">
<pre><strong>Input:</strong> board = [["a","b"],["c","d"]], words = ["abcb"]
<strong>Output:</strong> []
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>m == board.length</code></li>
	<li><code>n == board[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 12</code></li>
	<li><code>board[i][j]</code> is a lowercase English letter.</li>
	<li><code>1 &lt;= words.length &lt;= 3 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= words[i].length &lt;= 10</code></li>
	<li><code>words[i]</code> consists of lowercase English letters.</li>
	<li>All the strings of <code>words</code> are unique.</li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [String](https://leetcode.com/tag/string/), [Backtracking](https://leetcode.com/tag/backtracking/), [Trie](https://leetcode.com/tag/trie/), [Matrix](https://leetcode.com/tag/matrix/)

**Similar Questions**:
* [Word Search (Medium)](https://leetcode.com/problems/word-search/)
* [Unique Paths III (Hard)](https://leetcode.com/problems/unique-paths-iii/)

## Solution 1. Trie

Generating all the permutations of words starting from each cell and then finding if the wordsToBeSearched exist in those permutations will lead to TLE.
The clever way is to insert the wordsToBeSearched into a trie with marking the end of the words, and 
while traversing the matrix starting from each cell continue only if the current character in the traversal follows the any of the wordsToBeSearched path to its end,
And include the word if end is reached.

### Complexity Analysis
Building the Trie takes O(WL) time and O(WL) space, where W is the size of array words and L is the maximum length of a word.

The DFS part will take each point on the board as a starting point. There are O(MN) starting points. For each starting point, we DFS in 4 directions with the maximum depth being L. So the DFS part takes O(MN * 4^L) time.

```cpp
// OJ: https://leetcode.com/problems/word-search-ii/
// Author: A M A N
// Time : O(WL + MN*4^L)
// Space: O(WL)
class Solution {
public:
    struct TrieNode {
        bool end;
        unordered_map<char, TrieNode*> children;
        TrieNode(){
            end = false;
        }
    };
    void insert(TrieNode* head, string word){
        auto node = head;
        for(char c: word){
            if(node->children.find(c)==node->children.end())
                node->children[c] = new TrieNode();
            node = node->children[c];
        }
        node->end = true;
    }
    int n, m;
    bool isValid(int x, int y){
        if(x<0 or x>=n or y<0 or y>=m)
            return false;
        return true;
    }
        void dfs(vector<vector<char>>&board, int x, int y, string word, vector<string>& foundWords, TrieNode* root){
        if(!isValid(x, y) or board[x][y]==' ') return;
        if(root->children.find(board[x][y])==root->children.end())return; // current character in the traversal is not present in the trie 
        root = root->children[board[x][y]]; 
        word+=board[x][y];
        if(root->end){ // our trie contains the words which need to be searched, and this marks the end of one of those words
            foundWords.push_back(word);
            root->end = false; // to not recount the same found word in another traversal            
        }
        char temp = board[x][y];
        board[x][y] = ' ';
        dfs(board, x+1, y,    word, foundWords, root);
        dfs(board, x-1, y,    word, foundWords, root);
        dfs(board, x,   y+1,  word, foundWords, root);
        dfs(board, x,   y-1,  word, foundWords, root);
        board[x][y] = temp; // backtracking helps us to pass the board matrix by reference saving us a lot of space and time, instead of passing the matrix by value or using a visited matrix
    }
    vector<string> findWords(vector<vector<char>>& board, vector<string>& words) {
        n = board.size(), m = board[0].size();
        TrieNode* head = new TrieNode();
        for(auto s: words)
            insert(head, s); // inserting all the words to be searched in the trie
        // searching in the grid starting from each cell if in between traversal we reach trie->end == true
        vector<string> foundWords;
        for(int i=0; i<n; i++){
            for(int j=0; j<m; j++){
                dfs(board, i, j, "", foundWords, head);
            }
        }
        return foundWords;
    }
};
```