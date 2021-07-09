# [663. Walls and Gates (Medium)](https://www.lintcode.com/problem/663/)

<p>You are given a m x n 2D grid initialized with these three possible values.

<code>-1 - A wall or an obstacle.</code><br>
<code>-0 - A gate.</code> <br>
<code>-INF - Infinity means an empty room.</code>

 We use the value 2^31 - 1 = 2147483647 to represent INF as you may assume that the distance to a gate is less than 2147483647.
Fill each empty room with the distance to its nearest gate. If it is impossible to reach a Gate, that room should remain filled with INF</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<pre><strong>Input:</strong> grid = [[1,0,1],[0,0,0],[1,0,1]]
Input:
[[2147483647,-1,0,2147483647],[2147483647,2147483647,2147483647,-1],[2147483647,-1,2147483647,-1],[0,-1,2147483647,2147483647]]
Output:
[[3,-1,0,1],[2,2,1,-1],[1,-1,2,-1],[0,-1,3,4]]

Explanation:
the 2D grid is:
INF  -1  0  INF
INF INF INF  -1
INF  -1 INF  -1
  0  -1 INF INF
the answer is:
  3  -1   0   1
  2   2   1  -1
  1  -1   2  -1
  0  -1   3   4
</pre>



## Solution 1. BFS (Multi-centred)

```cpp
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