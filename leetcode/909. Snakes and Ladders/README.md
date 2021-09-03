# [909. Snakes and Ladders (Medium)](https://leetcode.com/problems/snakes-and-ladders/)

<p>You are given an <code>n x n</code> integer matrix <code>board</code> where the cells are labeled from <code>1</code> to <code>n<sup>2</sup></code> in a boustrophedon style starting from the bottom left of the board (i.e. <code>board[n - 1][0]</code>) and alternating direction each row.</p>

<p>You start on square <code>1</code> of the board. In each move, starting from square <code>x</code>, consists of the following:</p>

<ul>
	<li>You choose a destination square <code>S</code> with a label in the range <code>[x + 1, x + 6]</code> where <code>S &lt;= n<sup>2</sup></code>.

	<ul>
		<li>This choice simulates the result of a standard <strong>6-sided die roll</strong>: i.e., there are always at most size destinations, regardless of the size of the board.</li>
	</ul>
	</li>
	<li>If <code>S</code> has a snake or ladder, you move to the destination of that snake or ladder. Otherwise, you move to <code>S</code>.</li>
</ul>

<p>A board square on row <code>r</code> and column <code>c</code> has a snake or ladder if <code>board[r][c] != -1</code>. The destination of that snake or ladder is <code>board[r][c]</code>.</p>

<p>Note that you only take a snake or ladder at most once per move: if the destination to a snake or ladder is the start of another snake or ladder, you do not continue moving.</p>

<ul>
	<li>For example, if the board is <code>[[4,-1],[-1,3]]</code>&nbsp;and on the first move, your destination square is <code>2</code>, then you finish your first move at <code>3</code>, because you do not continue moving to <code>4</code>.</li>
</ul>

<p>Return <em>the least number of moves required to reach the square </em><code>n<sup>2</sup></code>. If it is not possible, return <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2018/09/23/snakes.png" style="width: 500px; height: 394px;">
<pre><strong>Input:</strong> board = [[-1,-1,-1,-1,-1,-1],[-1,-1,-1,-1,-1,-1],[-1,-1,-1,-1,-1,-1],[-1,35,-1,-1,13,-1],[-1,-1,-1,-1,-1,-1],[-1,15,-1,-1,-1,-1]]
<strong>Output:</strong> 4
<strong>Explanation:</strong> 
In the beginning, you start at square 1 [at row 5, column 0].
You decide to move to square 2 and must take the ladder to square 15.
You then decide to move to square 17 (row 3, column 4) and take the snake to square 13.
You then decide to move to square 14 and must take the ladder to square 35.
You then decide to move to square 36, ending the game.
It can be shown that you need at least 4 moves to reach the N*N-th square, so the answer is 4.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> board = [[-1,-1],[-1,3]]
<strong>Output:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>n == board.length</code></li>
	<li><code>n == board[i].length</code></li>
	<li><code>2 &lt;= n &lt;= 20</code></li>
	<li><code>grid[i][j]</code> is either <code>-1</code> or in the range <code>[1, n<sup>2</sup>]</code>.</li>
	<li>The squares labeled <code>1</code> and <code>n<sup>2</sup></code> does not have any ladders or snakes.</li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Breadth-First Search](https://leetcode.com/tag/breadth-first-search/), [Matrix](https://leetcode.com/tag/matrix/)

## Solution 1. BFS

```cpp
// OJ: https://leetcode.com/problems/snakes-and-ladders/
// Author: A M A N
// Time : O()
// Space: O()
class Solution {
public:
    int posToNum(int i, int j, int n){ //converts the boustrophedon style index to 1D index
        int res = (n-i-1)*n;
        if((n-i-1)&1^1) res+=j+1;
        else            res+=n-j;
        return res;
    }    
    int snakesAndLadders(vector<vector<int>>& board) {
        map<int, int> mp;
        int n = board.size();
        for(int i=0; i<board.size(); i++)
            for(int j=0; j<board[0].size(); j++)
                if(board[i][j]!=-1)
                    mp[posToNum(i, j, n)] = board[i][j];
        queue<int> q;
        q.push(1);
        int steps=0;
        unordered_set<int> vis;
        vis.insert(1);
        while(!q.empty()){
            for(int i=q.size(); i>0; i--){
                int cur = q.front(); q.pop();
                if(cur==n*n)    // game finished
                    return steps; 
                for(int k = 1; k<=6; k++){
                    int next = cur + k;
                    if(mp.find(next)!=mp.end()) // ladder or snake exist at the pos
                        next = mp[next];
                    if(next>n*n)continue;
                    if(!vis.count(next)){
                        vis.insert(next);
                        q.push(next);
                    }
                }
            }
            steps++;
        }
        return -1;
    }
};
```