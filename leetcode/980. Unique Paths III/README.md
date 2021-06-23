# [980. Unique Paths III (Hard)](https://leetcode.com/problems/unique-paths-iii/)

<p>On a 2-dimensional&nbsp;<code>grid</code>, there are 4 types of squares:</p>

<ul>
	<li><code>1</code> represents the starting square.&nbsp; There is exactly one starting square.</li>
	<li><code>2</code> represents the ending square.&nbsp; There is exactly one ending square.</li>
	<li><code>0</code> represents empty squares we can walk over.</li>
	<li><code>-1</code> represents obstacles that we cannot walk over.</li>
</ul>

<p>Return the number of 4-directional walks&nbsp;from the starting square to the ending square, that <strong>walk over every non-obstacle square&nbsp;exactly once</strong>.</p>

<p>&nbsp;</p>

<div>
<p><strong>Example 1:</strong></p>

<pre><strong>Input: </strong><span id="example-input-1-1">[[1,0,0,0],[0,0,0,0],[0,0,2,-1]]</span>
<strong>Output: </strong><span id="example-output-1">2</span>
<strong>Explanation: </strong>We have the following two paths: 
1. (0,0),(0,1),(0,2),(0,3),(1,3),(1,2),(1,1),(1,0),(2,0),(2,1),(2,2)
2. (0,0),(1,0),(2,0),(2,1),(1,1),(0,1),(0,2),(0,3),(1,3),(1,2),(2,2)</pre>

<div>
<p><strong>Example 2:</strong></p>

<pre><strong>Input: </strong><span id="example-input-2-1">[[1,0,0,0],[0,0,0,0],[0,0,0,2]]</span>
<strong>Output: </strong><span id="example-output-2">4</span>
<strong>Explanation: </strong>We have the following four paths: 
1. (0,0),(0,1),(0,2),(0,3),(1,3),(1,2),(1,1),(1,0),(2,0),(2,1),(2,2),(2,3)
2. (0,0),(0,1),(1,1),(1,0),(2,0),(2,1),(2,2),(1,2),(0,2),(0,3),(1,3),(2,3)
3. (0,0),(1,0),(2,0),(2,1),(2,2),(1,2),(1,1),(0,1),(0,2),(0,3),(1,3),(2,3)
4. (0,0),(1,0),(2,0),(2,1),(1,1),(0,1),(0,2),(0,3),(1,3),(1,2),(2,2),(2,3)</pre>

<div>
<p><strong>Example 3:</strong></p>

<pre><strong>Input: </strong><span id="example-input-3-1">[[0,1],[2,0]]</span>
<strong>Output: </strong><span id="example-output-3">0</span>
<strong>Explanation: </strong>
There is no path that walks over every empty square exactly once.
Note that the starting and ending square can be anywhere in the grid.
</pre>
</div>
</div>
</div>

<p>&nbsp;</p>

<p><strong>Note:</strong></p>

<ol>
	<li><code>1 &lt;= grid.length * grid[0].length &lt;= 20</code></li>
</ol>

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Backtracking](https://leetcode.com/tag/backtracking/), [Bit Manipulation](https://leetcode.com/tag/bit-manipulation/), [Matrix](https://leetcode.com/tag/matrix/)

**Similar Questions**:
* [Sudoku Solver (Hard)](https://leetcode.com/problems/sudoku-solver/)
* [Unique Paths II (Medium)](https://leetcode.com/problems/unique-paths-ii/)
* [Word Search II (Hard)](https://leetcode.com/problems/word-search-ii/)

## Solution 1. Backtracking

```cpp
// OJ: https://leetcode.com/problems/unique-paths-iii/
// Author: A M A N
// Time:   O(4^(MN))
// Space:  O(MN)
class Solution {
public:

    int ans;
    int n, m;
    int noOfZero;
    int dx[4] = {0, 1, 0, -1};
    int dy[4] = {1, 0, -1, 0};
    bool isValid(int x, int y){
        if(x<0 or x>=n or y<0 or y>=m)
            return false;
        return true;
    }
    int count(vector<vector<int>> &v, int x){
        int cnt=0;
        for(int i=0; i<v.size(); i++)
            for(int j=0; j<v[0].size(); j++)
                if(v[i][j]==x)
                    cnt++;
        return cnt;
    }
    void solve(vector<vector<int>>& grid, vector<vector<int>> vis, int x, int y, int zero ){
        if(grid[x][y]==2){
            // int noOfVis = count(vis, 1);
            // if(noOfVis== 2+noOfZero)
            //     ans++;
            if(zero==0)
                ans++;
            return;
        }
        for(int i=0; i<4; i++){
            int X = x + dx[i];
            int Y = y + dy[i];
            if(!isValid(X, Y)) continue;
            if(vis[X][Y])      continue;
            if(grid[X][Y]==-1) continue;
            vis[X][Y] = 1;
            solve(grid, vis, X, Y, zero-1);
            vis[X][Y] = 0;
        }
        return;
    }
    int uniquePathsIII(vector<vector<int>>& grid) {
        n = grid.size(), m = grid[0].size();
        noOfZero = count(grid, 0);
        for(int i=0; i<n; i++){
            for(int j=0; j<m; j++){
                if(grid[i][j]==1){
                    vector<vector<int>> vis(n, vector<int>(m ,false));
                    vis[i][j] = 1;
                    solve(grid, vis, i, j, noOfZero+1);
                    break;
                }
            }
        }
        return ans;
    }
};
```