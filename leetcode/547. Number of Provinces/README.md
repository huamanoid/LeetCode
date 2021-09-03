# [547. Number of Provinces (Medium)](https://leetcode.com/problems/number-of-provinces/)

<p>There are <code>n</code> cities. Some of them are connected, while some are not. If city <code>a</code> is connected directly with city <code>b</code>, and city <code>b</code> is connected directly with city <code>c</code>, then city <code>a</code> is connected indirectly with city <code>c</code>.</p>

<p>A <strong>province</strong> is a group of directly or indirectly connected cities and no other cities outside of the group.</p>

<p>You are given an <code>n x n</code> matrix <code>isConnected</code> where <code>isConnected[i][j] = 1</code> if the <code>i<sup>th</sup></code> city and the <code>j<sup>th</sup></code> city are directly connected, and <code>isConnected[i][j] = 0</code> otherwise.</p>

<p>Return <em>the total number of <strong>provinces</strong></em>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2020/12/24/graph1.jpg" style="width: 222px; height: 142px;">
<pre><strong>Input:</strong> isConnected = [[1,1,0],[1,1,0],[0,0,1]]
<strong>Output:</strong> 2
</pre>

<p><strong>Example 2:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2020/12/24/graph2.jpg" style="width: 222px; height: 142px;">
<pre><strong>Input:</strong> isConnected = [[1,0,0],[0,1,0],[0,0,1]]
<strong>Output:</strong> 3
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 200</code></li>
	<li><code>n == isConnected.length</code></li>
	<li><code>n == isConnected[i].length</code></li>
	<li><code>isConnected[i][j]</code> is <code>1</code> or <code>0</code>.</li>
	<li><code>isConnected[i][i] == 1</code></li>
	<li><code>isConnected[i][j] == isConnected[j][i]</code></li>
</ul>

**Related Topics**:  
[Depth-First Search](https://leetcode.com/tag/depth-first-search/), [Breadth-First Search](https://leetcode.com/tag/breadth-first-search/), [Union Find](https://leetcode.com/tag/union-find/), [Graph](https://leetcode.com/tag/graph/)

**Similar Questions**:
* [Number of Connected Components in an Undirected Graph (Medium)](https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/)
* [Robot Return to Origin (Easy)](https://leetcode.com/problems/robot-return-to-origin/)
* [Sentence Similarity (Easy)](https://leetcode.com/problems/sentence-similarity/)
* [Sentence Similarity II (Medium)](https://leetcode.com/problems/sentence-similarity-ii/)
* [The Earliest Moment When Everyone Become Friends (Medium)](https://leetcode.com/problems/the-earliest-moment-when-everyone-become-friends/)

## Solution 1. Union Find

```cpp
// OJ: https://leetcode.com/problems/number-of-provinces/
// Author: A M A N
// Time : O()
// Space: O()
class Solution {
public:
    const int mxN = 201;
    int p[201];
    int find(int u){
        while(p[u]!=-1)
            u = p[u];
        return u;
    }
    bool unite(int u, int v){
        if((u=find(u))==(v=find(v)))
            return false;
        p[u] = v;
        return true;
    }
    int findCircleNum(vector<vector<int>>& adj, int cnt=0) {
        memset(p, -1, sizeof p);
        for(int i=0;i<adj.size();i++)
        for(int j=0;j<i;j++)
            if(adj[i][j]){
                // unite(i,j);
                int u = find(i), v = find(j);
                if(u!=v)
                    p[u] = v;
            }
        for(int i=0; i<adj.size();i++){
            if(p[i]==-1)
                cnt++;
        }
        return cnt;
    }
};
```
## Solution 2. DFS

```cpp
// OJ: https://leetcode.com/problems/number-of-provinces/
// Author: A M A N
// Time : O(N^2)
// Space: O(N^2)
class Solution {
public:
    
    void dfs(vector<vector<int>>& adj, vector<bool>& vis, int u){
        vis[u] = 1;
        for(int v=0;v<adj.size();v++)
            if(adj[u][v] and !vis[v])
                dfs(adj, vis, v);
    }
    
    int findCircleNum(vector<vector<int>>& adj, int cnt=0) {
        vector<bool> vis(adj.size(), false);
        for(int i=0;i<adj.size();i++)
            if(!vis[i])
                dfs(adj, vis, i), cnt++;
        return cnt;
    }
};
```