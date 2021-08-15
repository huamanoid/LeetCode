# [773. Sliding Puzzle (Hard)](https://leetcode.com/problems/sliding-puzzle/)

<p>On an <code>2 x 3</code> board, there are five tiles labeled from <code>1</code> to <code>5</code>, and an empty square represented by <code>0</code>. A <strong>move</strong> consists of choosing <code>0</code> and a 4-directionally adjacent number and swapping it.</p>

<p>The state of the board is solved if and only if the board is <code>[[1,2,3],[4,5,0]]</code>.</p>

<p>Given the puzzle board <code>board</code>, return <em>the least number of moves required so that the state of the board is solved</em>. If it is impossible for the state of the board to be solved, return <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/06/29/slide1-grid.jpg" style="width: 244px; height: 165px;">
<pre><strong>Input:</strong> board = [[1,2,3],[4,0,5]]
<strong>Output:</strong> 1
<strong>Explanation:</strong> Swap the 0 and the 5 in one move.
</pre>

<p><strong>Example 2:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/06/29/slide2-grid.jpg" style="width: 244px; height: 165px;">
<pre><strong>Input:</strong> board = [[1,2,3],[5,4,0]]
<strong>Output:</strong> -1
<strong>Explanation:</strong> No number of moves will make the board solved.
</pre>

<p><strong>Example 3:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/06/29/slide3-grid.jpg" style="width: 244px; height: 165px;">
<pre><strong>Input:</strong> board = [[4,1,2],[5,0,3]]
<strong>Output:</strong> 5
<strong>Explanation:</strong> 5 is the smallest number of moves that solves the board.
An example path:
After move 0: [[4,1,2],[5,0,3]]
After move 1: [[4,1,2],[0,5,3]]
After move 2: [[0,1,2],[4,5,3]]
After move 3: [[1,0,2],[4,5,3]]
After move 4: [[1,2,0],[4,5,3]]
After move 5: [[1,2,3],[4,5,0]]
</pre>

<p><strong>Example 4:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/06/29/slide4-grid.jpg" style="width: 244px; height: 165px;">
<pre><strong>Input:</strong> board = [[3,2,4],[1,5,0]]
<strong>Output:</strong> 14
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>board.length == 2</code></li>
	<li><code>board[i].length == 3</code></li>
	<li><code>0 &lt;= board[i][j] &lt;= 5</code></li>
	<li>Each value <code>board[i][j]</code> is <strong>unique</strong>.</li>
</ul>


**Companies**:  
[Uber](https://leetcode.com/company/uber)

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Breadth-First Search](https://leetcode.com/tag/breadth-first-search/), [Matrix](https://leetcode.com/tag/matrix/)

## Solution 1. Backtracking (DFS) with Memoization 

`Only visited state` info doesnt help because we might visit some states `first` with more number of steps than optimal, and since
we've visited it, we won't be visiting it again. So, reaching the solved state with non optimal steps and thus incorrect answer.


```cpp
// OJ: https://leetcode.com/problems/sliding-puzzle/
// Author: A M A N
// Time : O(1)
// Space: O(1)
class Solution {
public:
    int n = 2, m = 3;
    int dx[4] = {0, 1, 0, -1};
    int dy[4] = {1, 0, -1, 0};
    vector<vector<int>> solved{{1, 2, 3},{4, 5, 0}};
    int ans = 1e9;

    // set<vector<vector<int>>> vis;
    map<vector<vector<int>>, int> mp; //minimum steps to reach current state
    void solve(vector<vector<int>>&board, int temp){ // we can pass board matrix by reference as after modifying the board we're backtracking to the original
        if(temp > ans)return; //optimization
        if(board==solved){   
            ans = min(ans, temp);
            return;
        }
        if(mp.find(board)!=mp.end())
            if(mp[board]<temp)return; // there's a way to reach the current state in lesser steps
        mp[board] = temp; // temp is already the least steps to reach the current state
        for(int i=0; i<board.size(); i++){
            for(int j=0; j<board[0].size(); j++){
                if(board[i][j]==0){
                    for(int k = 0; k<4; k++){
                        int X = i + dx[k];
                        int Y = j + dy[k];
                        if(X<0 or X>=n or Y<0 or Y>=m) continue;
                        swap(board[i][j], board[X][Y]);
                        solve(board, temp+1);
                        swap(board[i][j], board[X][Y]); //backtrack
                    }
                }
            }
        }
        return;
    }
    int slidingPuzzle(vector<vector<int>>& board) {
        solve(board, 0);
        return ans==1e9?-1:ans;
    }
};
```
## Solution 2. BFS


`Only visited state` info is enough since we're visiting each state optimally.

```cpp
// OJ: https://leetcode.com/problems/sliding-puzzle/
// Author: A M A N
// Time : O(1)
// Space: O(1)
class Solution {
public:
    int dx[4] = {0, 1, 0, -1};
    int dy[4] = {1, 0, -1, 0};
    int slidingPuzzle(vector<vector<int>>& board) {
        int n = 2, m = 3;
        vector<vector<int>> solved{{1,2,3},{4,5,0}};
        set<vector<vector<int>>> vis;
        queue<vector<vector<int>>> q;
        q.push(board);
        int steps = 0;
        while(!q.empty()){
            steps++;
            int N = q.size();
            while(N--){
                vector<vector<int>> cur = q.front(); q.pop();
                if(cur==solved) return steps-1;
                vis.insert(cur);
                for(int i=0; i<n; i++)
                    for(int j=0; j<m; j++)
                        if(cur[i][j]==0){
                            for(int k = 0; k<4; k++){
                                int X = i + dx[k];
                                int Y = j + dy[k];
                                if(X<0 or Y<0 or X>=n or Y>=m) continue;
                                swap(cur[i][j], cur[X][Y]);
                                if(!vis.count(cur))
                                    q.push(cur);
                                swap(cur[i][j], cur[X][Y]);
                            }
                        }
            }
        }
        return -1;
    }
};
```
