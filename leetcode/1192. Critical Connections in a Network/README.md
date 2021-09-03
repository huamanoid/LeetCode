# [1192. Critical Connections in a Network (Hard)](https://leetcode.com/problems/critical-connections-in-a-network/)

<p>There are <code>n</code> servers numbered from <code>0</code> to <code>n - 1</code> connected by undirected server-to-server <code>connections</code> forming a network where <code>connections[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> represents a connection between servers <code>a<sub>i</sub></code> and <code>b<sub>i</sub></code>. Any server can reach other servers directly or indirectly through the network.</p>

<p>A <em>critical connection</em> is a connection that, if removed, will make some servers unable to reach some other server.</p>

<p>Return all critical connections in the network in any order.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<p><strong><img alt="" src="https://assets.leetcode.com/uploads/2019/09/03/1537_ex1_2.png" style="width: 198px; height: 248px;"></strong></p>

<pre><strong>Input:</strong> n = 4, connections = [[0,1],[1,2],[2,0],[1,3]]
<strong>Output:</strong> [[1,3]]
<strong>Explanation:</strong> [[3,1]] is also accepted.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>n - 1 &lt;= connections.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt;= n - 1</code></li>
	<li><code>a<sub>i</sub> != b<sub>i</sub></code></li>
	<li>There are no repeated connections.</li>
</ul>


**Related Topics**:  
[Depth-First Search](https://leetcode.com/tag/depth-first-search/), [Graph](https://leetcode.com/tag/graph/), [Biconnected Component](https://leetcode.com/tag/biconnected-component/)

## Solution 1. DFS - Tarjan's Algorithm

```cpp
// OJ: https://leetcode.com/problems/critical-connections-in-a-network/
// Author: A M A N
// Time : O(V + E)
// Space: O(V)
// Ref  : https://www.youtube.com/watch?v=RYaakWv5m6o
class Solution {
public:
    vector<vector<int>> bridge;
    // Tarjan's Algorithm 
    void dfs(vector<vector<int>>& adj, int u, int parent, int& time, vector<int>&d, vector<int>&l){
        d[u] = l[u] = time++;
        for(auto v: adj[u]){
            if(d[v]==-1){
                parent = u;
                dfs(adj, v, parent, time, d, l);
                // l[v] <= d[u] means there's a back edge(not the current edge) joining the child to parent
                if(l[v] > d[u]) // condition for Articulation point (no back-edge)
                    bridge.push_back({u, v});
                l[u] = min(l[v], l[u]);
            }else if(v!=parent){
                l[u] = min(l[v], l[u]);
            }
        }
    }
    vector<vector<int>> criticalConnections(int n, vector<vector<int>>& connections) {
        vector<vector<int>> adj(n);
        for(auto& e: connections){
            adj[e[0]].push_back(e[1]);
            adj[e[1]].push_back(e[0]);
        }
        vector<int> d(n, -1); // discovery time 
        vector<int> l(n, -1); // lowest "d" reachable from the node
        int parent=-1;
        int time = 0;
        dfs(adj, 0, parent, time, d, l);
        return bridge;
    }
};
```