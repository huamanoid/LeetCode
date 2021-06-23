# [79. Word Search (Medium)](https://leetcode.com/problems/word-search/)

<p>Given an <code>m x n</code> grid of characters <code>board</code> and a string <code>word</code>, return <code>true</code> <em>if</em> <code>word</code> <em>exists in the grid</em>.</p>

<p>The word can be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2020/11/04/word2.jpg" style="width: 322px; height: 242px;">
<pre><strong>Input:</strong> board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
<strong>Output:</strong> true
</pre>

<p><strong>Example 2:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2020/11/04/word-1.jpg" style="width: 322px; height: 242px;">
<pre><strong>Input:</strong> board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "SEE"
<strong>Output:</strong> true
</pre>

<p><strong>Example 3:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2020/10/15/word3.jpg" style="width: 322px; height: 242px;">
<pre><strong>Input:</strong> board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCB"
<strong>Output:</strong> false
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>m == board.length</code></li>
	<li><code>n = board[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 6</code></li>
	<li><code>1 &lt;= word.length &lt;= 15</code></li>
	<li><code>board</code> and <code>word</code> consists of only lowercase and uppercase English letters.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Follow up:</strong> Could you use search pruning to make your solution faster with a larger <code>board</code>?</p>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Backtracking](https://leetcode.com/tag/backtracking/), [Matrix](https://leetcode.com/tag/matrix/)

**Similar Questions**:
* [Word Search II (Hard)](https://leetcode.com/problems/word-search-ii/)

## Solution 1. Backtracking

```cpp
// OJ: https://leetcode.com/problems/word-search/
// Author: A M A N
// Time: O()
// Space: O()
class Solution {
public:
    int dx[4] = {0,1,0,-1};
    int dy[4] = {1,0,-1,0};
    bool solve(vector<vector<char>> board, vector<vector<int>> vis, string word, int pos,  int x, int y){
        if(word.size()==pos){
            return true;
        }
        for(int i=0; i<4; i++){
            int X = x + dx[i];
            int Y = y + dy[i];
            if(X<0 or X>=board.size() or Y<0 or Y>=board[0].size()) continue;
            if(vis[X][Y]) continue;
            if(board[X][Y]!=word[pos])  continue;
            vis[X][Y] = 1;
            if(solve(board, vis, word, pos+1, X, Y))
                return true;
            vis[X][Y] = 0;
        }
        return false;
    }
    bool exist(vector<vector<char>>& board, string word) {
        set<char> sett;
        for(auto s:board)
            for(char c: s)
                sett.insert(c);
        for(char c: word)
            if(sett.find(c)==sett.end())
                return false;
        for(int i=0; i<board.size(); i++)
            for(int j=0; j<board[0].size(); j++)
                if(board[i][j]==word[0]){
                    vector<vector<int>> vis(board.size(), vector<int>(board[0].size(), false));
                    vis[i][j] = 1;
                    if(solve(board, vis, word, 1, i, j))
                         return true;
                    vis[i][j] = 0;
                }
        return false;
    }
};
```