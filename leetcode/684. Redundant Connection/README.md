# [684. Redundant Connection (Medium)](https://leetcode.com/problems/redundant-connection/)

<p>In this problem, a tree is an <strong>undirected graph</strong> that is connected and has no cycles.</p>

<p>You are given a graph that started as a tree with <code>n</code> nodes labeled from <code>1</code> to <code>n</code>, with one additional edge added. The added edge has two <strong>different</strong> vertices chosen from <code>1</code> to <code>n</code>, and was not an edge that already existed. The graph is represented as an array <code>edges</code> of length <code>n</code> where <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> indicates that there is an edge between nodes <code>a<sub>i</sub></code> and <code>b<sub>i</sub></code> in the graph.</p>

<p>Return <em>an edge that can be removed so that the resulting graph is a tree of </em><code>n</code><em> nodes</em>. If there are multiple answers, return the answer that occurs last in the input.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/05/02/reduntant1-1-graph.jpg" style="width: 222px; height: 222px;">
<pre><strong>Input:</strong> edges = [[1,2],[1,3],[2,3]]
<strong>Output:</strong> [2,3]
</pre>

<p><strong>Example 2:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/05/02/reduntant1-2-graph.jpg" style="width: 382px; height: 222px;">
<pre><strong>Input:</strong> edges = [[1,2],[2,3],[3,4],[1,4],[1,5]]
<strong>Output:</strong> [1,4]
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>n == edges.length</code></li>
	<li><code>3 &lt;= n &lt;= 1000</code></li>
	<li><code>edges[i].length == 2</code></li>
	<li><code>1 &lt;= a<sub>i</sub> &lt; b<sub>i</sub> &lt;= edges.length</code></li>
	<li><code>a<sub>i</sub> != b<sub>i</sub></code></li>
	<li>There are no repeated edges.</li>
	<li>The given graph is connected.</li>
</ul>


**Related Topics**:  
[Depth-First Search](https://leetcode.com/tag/depth-first-search/), [Breadth-First Search](https://leetcode.com/tag/breadth-first-search/), [Union Find](https://leetcode.com/tag/union-find/), [Graph](https://leetcode.com/tag/graph/)

**Similar Questions**:
* [Redundant Connection II (Hard)](https://leetcode.com/problems/redundant-connection-ii/)
* [Accounts Merge (Medium)](https://leetcode.com/problems/accounts-merge/)

## Solution 1. Union Find

```cpp
// OJ: https://leetcode.com/problems/redundant-connection/
// Author: A M A N
// Time : O()
// Space: O()
class Solution {
public:
    int p[1001];
    int find(int u){
        while(p[u]!=-1)
            u = p[u];
        return u;
    }
    vector<int> findRedundantConnection(vector<vector<int>>& edges) {
        memset(p, -1, sizeof p);
        for(auto &e: edges){
            int u = find(e[0]), v = find(e[1]);
            if(u!=v)
                p[u] = v;
            else
                return e;
        }
        return {};
    }
};
```
## Solution 2. DFS

```cpp
// OJ: https://leetcode.com/problems/redundant-connection/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    vector<int> adj[1001];
    bool dfs(int u, int v, int pre=-1){ // can use vis array but we just have to find once where its visited
        if(u==v)
            return true;
        for(int cu: adj[u]){
            if(cu==pre)continue;
            if(dfs(cu, v, u))
                return true;
        }
        return false;
    }
    vector<int> findRedundantConnection(vector<vector<int>>& edges) {
        for(auto e: edges){
            if(dfs(e[0], e[1]))
                return e; // only 1 extra edges is added
            else{
                adj[e[0]].push_back(e[1]);
                adj[e[1]].push_back(e[0]);
            }
        }
        return {};
    }
};
```