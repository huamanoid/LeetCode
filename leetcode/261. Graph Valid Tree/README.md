# [261. Graph Valid Tree (Medium)](https://leetcode.com/problems/graph-valid-tree/)

<p>You have a graph of <code>n</code> nodes labeled from <code>0</code> to <code>n - 1</code>. You are given an integer n and a list of <code>edges</code> where <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> indicates that there is an undirected edge between nodes <code>a<sub>i</sub></code> and <code>b<sub>i</sub></code> in the graph.</p>

<p>Return <code>true</code> <em>if the edges of the given graph make up a valid tree, and</em> <code>false</code> <em>otherwise</em>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/03/12/tree1-graph.jpg" style="width: 222px; height: 302px;">
<pre><strong>Input:</strong> n = 5, edges = [[0,1],[0,2],[0,3],[1,4]]
<strong>Output:</strong> true
</pre>

<p><strong>Example 2:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/03/12/tree2-graph.jpg" style="width: 382px; height: 222px;">
<pre><strong>Input:</strong> n = 5, edges = [[0,1],[1,2],[2,3],[1,3],[1,4]]
<strong>Output:</strong> false
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= 2000 &lt;= n</code></li>
	<li><code>0 &lt;= edges.length &lt;= 5000</code></li>
	<li><code>edges[i].length == 2</code></li>
	<li><code>0 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt; n</code></li>
	<li><code>a<sub>i</sub> != b<sub>i</sub></code></li>
	<li>There are no self-loops or repeated edges.</li>
</ul>


**Companies**:  
[LinkedIn](https://leetcode.com/company/linkedin), [Microsoft](https://leetcode.com/company/microsoft), [Amazon](https://leetcode.com/company/amazon), [Google](https://leetcode.com/company/google)

**Related Topics**:  
[Depth-First Search](https://leetcode.com/tag/depth-first-search/), [Breadth-First Search](https://leetcode.com/tag/breadth-first-search/), [Union Find](https://leetcode.com/tag/union-find/), [Graph](https://leetcode.com/tag/graph/)

**Similar Questions**:
* [Course Schedule (Medium)](https://leetcode.com/problems/course-schedule/)
* [Number of Connected Components in an Undirected Graph (Medium)](https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/)
* [Keys and Rooms (Medium)](https://leetcode.com/problems/keys-and-rooms/)

## Solution 1. DFS
 
```cpp
// OJ: https://leetcode.com/problems/graph-valid-tree/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    bool vis[2001];
    bool dfs(vector<vector<int>>&adj, int u, int pu){
        if(vis[u]) return false;
        vis[u] = 1;
        for(auto v: adj[u]){
            if(v!=pu and !dfs(adj, v, u))
                return false;
        }
        return true;
    }
    bool validTree(int n, vector<vector<int>>& edges) {
        memset(vis, 0, 0);
        vector<vector<int>>adj(n);
        for(auto v: edges){
            adj[v[0]].push_back(v[1]); // undirected edges
            adj[v[1]].push_back(v[0]);
        }
        if(!dfs(adj, 0, 0)) // to take care of cycles
            return false;
        int visitedNodes = accumulate(vis,vis+n, 0);
        return visitedNodes==n;// to take care of disconnectedness
    }
};
```

Tree of N nodes must have `N-1 edges` to be a valid tree (no loops and connected).
We can just check the no of edges in the first place to take care of cyclicity.

```cpp
// OJ: https://leetcode.com/problems/graph-valid-tree/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    bool vis[2001];
    void dfs(vector<vector<int>>&adj, int u, int pu){
        if(vis[u]) return;
        vis[u] = 1;
        for(auto v: adj[u]){
            if(v!=pu)
                dfs(adj, v, u);
        }
    }
    bool validTree(int n, vector<vector<int>>& edges) {
        memset(vis, 0, 0);
        if(edges.size()!=n-1)return false; // to take care of cycles
        vector<vector<int>>adj(n);
        for(auto v: edges){
            adj[v[0]].push_back(v[1]); // undirected edges
            adj[v[1]].push_back(v[0]);
        }
        dfs(adj, 0, -1); // to mark visited nodes
        int visitedNodes = accumulate(vis,vis+n, 0);
        return visitedNodes==n;// to take care of disconnectedness
    }
};
```

## Solution 2. Union-Find

```cpp
// OJ: https://leetcode.com/problems/graph-valid-tree/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    int p[2001];
    int find(int u){
        while(p[u]!=u){
            p[u] = p[p[u]]; // path compression
            u = p[u];
        }
        return p[u];
    }
    bool validTree(int n, vector<vector<int>>& edges) {
        iota(p, p+n, 0);
        for(auto e: edges){
            int u = find(e[0]);
            int v = find(e[1]);
            if(u==v) // even without the current there's a path between these two nodes
                return false; // so this is a redundant edge, forming cycle
            p[u] = v; //unite
        }
        return edges.size()==n-1; 
    }
};
```

## Solution 3. BFS

```cpp
// OJ: https://leetcode.com/problems/graph-valid-tree/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    bool validTree(int n, vector<vector<int>>& edges) {
        if(edges.size()!=n-1)return false; 
        vector<vector<int>> adj(n);
        for(auto e: edges){
            adj[e[0]].push_back(e[1]);
            adj[e[1]].push_back(e[0]);
        }
        vector<bool>vis(n, false);
        queue<pair<int, int>> q;
        q.push({0, -1});
        vis[0] = 1;
        while(!q.empty()){
            auto [u, pu] = q.front(); q.pop();
            for(auto v: adj[u]){
                if(v==pu)continue;
                if(vis[v])return false; // cycle detected
                vis[v] = 1; 
                q.push({v, u});
            }
        }
        int visitedNodes = accumulate(vis.begin(), vis.end(), 0); 
        return visitedNodes==n; // to take care of disconnectedness, as we're visitig only the set which can be reached from 0
    }
};
```