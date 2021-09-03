# [286. Walls and Gates (Medium)](https://leetcode.com/problems/walls-and-gates/)

<p>You are given an <code>m x n</code> grid <code>rooms</code>&nbsp;initialized with these three possible values.</p>

<ul>
	<li><code>-1</code>&nbsp;A wall or an obstacle.</li>
	<li><code>0</code> A gate.</li>
	<li><code>INF</code> Infinity means an empty room. We use the value <code>2<sup>31</sup> - 1 = 2147483647</code> to represent <code>INF</code> as you may assume that the distance to a gate is less than <code>2147483647</code>.</li>
</ul>

<p>Fill each empty room with the distance to <em>its nearest gate</em>. If it is impossible to reach a gate, it should be filled with <code>INF</code>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/01/03/grid.jpg" style="width: 500px; height: 223px;">
<pre><strong>Input:</strong> rooms = [[2147483647,-1,0,2147483647],[2147483647,2147483647,2147483647,-1],[2147483647,-1,2147483647,-1],[0,-1,2147483647,2147483647]]
<strong>Output:</strong> [[3,-1,0,1],[2,2,1,-1],[1,-1,2,-1],[0,-1,3,4]]
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> rooms = [[-1]]
<strong>Output:</strong> [[-1]]
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> rooms = [[2147483647]]
<strong>Output:</strong> [[2147483647]]
</pre>

<p><strong>Example 4:</strong></p>

<pre><strong>Input:</strong> rooms = [[0]]
<strong>Output:</strong> [[0]]
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>m == rooms.length</code></li>
	<li><code>n == rooms[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 250</code></li>
	<li><code>rooms[i][j]</code> is <code>-1</code>, <code>0</code>, or <code>2<sup>31</sup> - 1</code>.</li>
</ul>


**Companies**:  
[Amazon](https://leetcode.com/company/amazon), [Spotify](https://leetcode.com/company/spotify)

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Breadth-First Search](https://leetcode.com/tag/breadth-first-search/), [Matrix](https://leetcode.com/tag/matrix/)

**Similar Questions**:
* [Surrounded Regions (Medium)](https://leetcode.com/problems/surrounded-regions/)
* [Number of Islands (Medium)](https://leetcode.com/problems/number-of-islands/)
* [Shortest Distance from All Buildings (Hard)](https://leetcode.com/problems/shortest-distance-from-all-buildings/)
* [Robot Room Cleaner (Hard)](https://leetcode.com/problems/robot-room-cleaner/)
* [Rotting Oranges (Medium)](https://leetcode.com/problems/rotting-oranges/)

## Solution 1. BFS (Multi-centred)

```cpp
// OJ: https://leetcode.com/problems/walls-and-gates/
// Author: A M A N
// Time : O(N*M)
// Space: O(N*M)
class Solution {
public:
    void wallsAndGates(vector<vector<int>> &rooms) {
        queue<pair<int, int>> q;
        for(int i=0; i<rooms.size(); i++){
            for(int j=0; j<rooms[i].size(); j++)
                if(rooms[i][j]==0)
                    q.push({i, j});
        }
        int dx[4] = {0, 0, -1, 1};
        int dy[4] = {1, -1, 0, 0};
        while(!q.empty()){
            int x = q.front().first, y = q.front().second; q.pop();
            for(int i=0; i<4; i++){
                int X = x + dx[i];
                int Y = y + dy[i];
                if(X<0 or Y<0 or X>=rooms.size() or Y>=rooms[0].size())continue;
                if(rooms[X][Y]!=INT_MAX)continue;
                rooms[X][Y] = rooms[x][y] + 1;
                q.push({X, Y});
            }
        }
    }
};
```