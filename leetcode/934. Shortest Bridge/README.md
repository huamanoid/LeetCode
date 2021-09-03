# [934. Shortest Bridge (Medium)](https://leetcode.com/problems/shortest-bridge/)

<p>In a given 2D binary array <code>grid</code>, there are two islands.&nbsp; (An island is a 4-directionally connected group of&nbsp;<code>1</code>s not connected to any other 1s.)</p>

<p>Now, we may change <code>0</code>s to <code>1</code>s so as to connect the two islands together to form 1 island.</p>

<p>Return the smallest number of <code>0</code>s that must be flipped.&nbsp; (It is guaranteed that the answer is at least 1.)</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> grid = [[0,1],[1,0]]
<strong>Output:</strong> 1
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> grid = [[0,1,0],[0,0,0],[0,0,1]]
<strong>Output:</strong> 2
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> grid = [[1,1,1,1,1],[1,0,0,0,1],[1,0,1,0,1],[1,0,0,0,1],[1,1,1,1,1]]
<strong>Output:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>2 &lt;= grid.length == grid[0].length &lt;= 100</code></li>
	<li><code>grid[i][j] == 0</code> or <code>grid[i][j] == 1</code></li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Depth-First Search](https://leetcode.com/tag/depth-first-search/), [Breadth-First Search](https://leetcode.com/tag/breadth-first-search/), [Matrix](https://leetcode.com/tag/matrix/)

## Solution 1. DFS + Brute Force 

```cpp
// OJ: https://leetcode.com/problems/shortest-bridge/
// Author: A M A N
// Time : O(N^4)
// Space: O(N^2)
class Solution {
public:
    struct SimpleHash {
        size_t operator()(const std::pair<int, int>& p) const {
            return p.first ^ p.second;
        }
    };
    void dfs(vector<vector<int>>&grid, vector<vector<bool>>&vis, int x, int y, unordered_set<pair<int,int>, SimpleHash>& sett){
        if(x<0 or y<0 or x>=grid.size() or y>=grid[0].size() or vis[x][y] or !grid[x][y]) return;
        sett.insert({x, y});
        vis[x][y] = 1;
        dfs(grid, vis, x+1, y, sett);
        dfs(grid, vis, x-1, y, sett);
        dfs(grid, vis, x, y+1, sett);
        dfs(grid, vis, x, y-1, sett);
    }
    int shortestBridge(vector<vector<int>>& grid) {
        int n = grid.size();
        int m = grid[0].size();
        unordered_set<pair<int, int>, SimpleHash> a, b; //contains {x, y} of diff islands
        vector<vector<bool>>vis(n, vector<bool>(m, false));
        for(int i=0; i<n; i++){
            for(int j=0; j<m; j++){
                if(!vis[i][j] and grid[i][j]){
                    if(a.empty())
                        dfs(grid, vis, i, j, a);
                    if(b.empty())
                        dfs(grid, vis, i, j, b);
                }
            }
        }
        int ans = INT_MAX;
        for(auto [x1, y1]: a){
            for(auto [x2, y2]: b){
                ans = min(ans, abs(x1-x2) + abs(y1-y2) - 1);
            }
        }
        return ans;
    }
};
```

## Solution 2. DFS + BFS

```cpp
// OJ: https://leetcode.com/problems/shortest-bridge/
// Author: A M A N
// Time : O(N^3)
// Space: O(N^2)
class Solution {
public:
    void dfs(vector<vector<int>>&grid, int x, int y){
        if(x<0 or y<0 or x>=grid.size() or y>=grid[0].size() or grid[x][y]!=1) return;
        grid[x][y] = 2;
        dfs(grid, x+1, y);
        dfs(grid, x-1, y);
        dfs(grid, x, y+1);
        dfs(grid, x, y-1);
    }
    bool expand(vector<vector<int>>&grid, int x, int y, int cl){
        if(x<0 or y<0 or x>=grid.size() or y>=grid[0].size()) return false;
        if(grid[x][y]==0)
            grid[x][y] = cl+1;
        return grid[x][y]==1; //returns true when the destination island is found,
    }
    int shortestBridge(vector<vector<int>>& grid) {
        for(int i=0, found=0; !found and i<grid.size(); i++){
            for(int j=0; !found and j<grid[0].size(); j++){
                if(grid[i][j]){
                    dfs(grid, i, j); // marking one of the islands as 2
                    found = 1;
                    // break; break will not help as it will break only one interior loop
                }
            }
        }
        for(int cl=2; ;cl++){ //breadth of the search
            for(int i=0; i<grid.size(); i++){
                for(int j=0; j<grid[0].size(); j++){
                    if(grid[i][j]==cl){
                        if(expand(grid, i+1, j, cl) or
                          expand(grid, i-1, j, cl) or
                          expand(grid, i, j+1, cl) or
                          expand(grid, i, j-1, cl)) //returns true when the the destination island is met
                            return cl-2; 
                    }
                }
            }
        }
        return 0;
    }
};
```

## Solution 3. DFS + BFS

```cpp
// OJ: https://leetcode.com/problems/shortest-bridge/
// Author: A M A N
// Time : O()
// Space: O()
class Solution {
public:
    queue<pair<int, int>> q;
    int dx[4] = {0,1,0,-1};
    int dy[4] = {1,0,-1, 0};
    void dfs(vector<vector<int>>&grid, int x, int y){
        if(x<0 or y<0 or x>=grid.size() or y>=grid[0].size() or grid[x][y]!=1) return;
        grid[x][y] = 2;
        q.push({x, y});
        dfs(grid, x+1, y);
        dfs(grid, x-1, y);
        dfs(grid, x, y+1);
        dfs(grid, x, y-1);
    }
    int shortestBridge(vector<vector<int>>& grid) {
        for(int i=0, found=0; !found and i<grid.size(); i++){
            for(int j=0; !found and j<grid[0].size(); j++){
                if(grid[i][j]){
                    dfs(grid, i, j); // marking one of the islands as 2
                    found = 1;
                    // break; break will not help as it will break only one interior loop
                }
            }
        }
        int breadth = 0;
        while(!q.empty()){
            int n = q.size();
            for(int i=0; i<n; i++){
                auto [x, y] = q.front(); q.pop();
                for(int j=0; j<4; j++){
                    int X = x + dx[j];
                    int Y = y + dy[j];
                    if(X<0 or Y<0 or X>=grid.size() or Y>=grid[0].size())continue; //check out of bounds
                    if(grid[X][Y]==2)continue; // already visited
                    if(grid[X][Y]==1)
                        return breadth;
                    grid[X][Y] = 2;
                    q.push({X, Y});
                }
            }
            breadth++;
        }
        return breadth;
    }
};
```