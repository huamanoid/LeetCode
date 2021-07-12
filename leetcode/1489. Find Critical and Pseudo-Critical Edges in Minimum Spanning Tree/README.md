# [1489. Find Critical and Pseudo-Critical Edges in Minimum Spanning Tree (Hard)](https://leetcode.com/problems/find-critical-and-pseudo-critical-edges-in-minimum-spanning-tree/)

<p>Given a weighted undirected connected graph with <code>n</code>&nbsp;vertices numbered from <code>0</code> to <code>n - 1</code>,&nbsp;and an array <code>edges</code>&nbsp;where <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>, weight<sub>i</sub>]</code> represents a bidirectional and weighted edge between nodes&nbsp;<code>a<sub>i</sub></code>&nbsp;and <code>b<sub>i</sub></code>. A minimum spanning tree (MST) is a subset of the graph's edges that connects all vertices without cycles&nbsp;and with the minimum possible total edge weight.</p>

<p>Find <em>all the critical and pseudo-critical edges in the given graph's minimum spanning tree (MST)</em>. An MST edge whose deletion from the graph would cause the MST weight to increase is called a&nbsp;<em>critical edge</em>. On&nbsp;the other hand, a pseudo-critical edge is that which can appear in some MSTs but not all.</p>

<p>Note that you can return the indices of the edges in any order.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<p><img alt="" src="https://assets.leetcode.com/uploads/2020/06/04/ex1.png" style="width: 259px; height: 262px;"></p>

<pre><strong>Input:</strong> n = 5, edges = [[0,1,1],[1,2,1],[2,3,2],[0,3,2],[0,4,3],[3,4,3],[1,4,6]]
<strong>Output:</strong> [[0,1],[2,3,4,5]]
<strong>Explanation:</strong> The figure above describes the graph.
The following figure shows all the possible MSTs:
<img alt="" src="https://assets.leetcode.com/uploads/2020/06/04/msts.png" style="width: 540px; height: 553px;">
Notice that the two edges 0 and 1 appear in all MSTs, therefore they are critical edges, so we return them in the first list of the output.
The edges 2, 3, 4, and 5 are only part of some MSTs, therefore they are considered pseudo-critical edges. We add them to the second list of the output.
</pre>

<p><strong>Example 2:</strong></p>

<p><img alt="" src="https://assets.leetcode.com/uploads/2020/06/04/ex2.png" style="width: 247px; height: 253px;"></p>

<pre><strong>Input:</strong> n = 4, edges = [[0,1,1],[1,2,1],[2,3,1],[0,3,1]]
<strong>Output:</strong> [[],[0,1,2,3]]
<strong>Explanation:</strong> We can observe that since all 4 edges have equal weight, choosing any 3 edges from the given 4 will yield an MST. Therefore all 4 edges are pseudo-critical.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 100</code></li>
	<li><code>1 &lt;= edges.length &lt;= min(200, n * (n - 1) / 2)</code></li>
	<li><code>edges[i].length == 3</code></li>
	<li><code>0 &lt;= a<sub>i</sub> &lt; b<sub>i</sub> &lt; n</code></li>
	<li><code>1 &lt;= weight<sub>i</sub>&nbsp;&lt;= 1000</code></li>
	<li>All pairs <code>(a<sub>i</sub>, b<sub>i</sub>)</code> are <strong>distinct</strong>.</li>
</ul>


**Related Topics**:  
[Union Find](https://leetcode.com/tag/union-find/), [Graph](https://leetcode.com/tag/graph/), [Sorting](https://leetcode.com/tag/sorting/), [Minimum Spanning Tree](https://leetcode.com/tag/minimum-spanning-tree/), [Strongly Connected Component](https://leetcode.com/tag/strongly-connected-component/)

## Solution 1. Kruskal's Algorithm

```cpp
// OJ: https://leetcode.com/problems/find-critical-and-pseudo-critical-edges-in-minimum-spanning-tree/
// Author: A M A N
// Time : O(ElogE + E^2)
// Space: O(V)
class Solution {
public:
    int N;
    unordered_map<int, int> p, sz;
    int find(int u){
        if(p.find(u)==p.end())
            p[u] = u, sz[u]=1;
        while(p[u]!=u){
            p[u] = p[p[u]]; //path compression
            u = p[u];
        }
        return u;
    }
    int MST(int n, vector<vector<int>>& edges, int include=-1, int exclude=-1){
        p.clear();
        sz.clear();
        int res=0;
        if(include!=-1){
            int u = edges[include][0], v = edges[include][1], w = edges[include][2];
            u = find(u);
            v = find(v);
            p[u]=v;
            res+=w;
        }
        for(int i=0; i<edges.size(); i++){
            if(i==exclude)continue;            
            int u = edges[i][0], v = edges[i][1], w = edges[i][2];
            u = find(u);
            v = find(v);
            if(u!=v){
                if(sz[u]>sz[v]) p[v] = u, sz[u]+=sz[v];
                else            p[u] = v, sz[v]+=sz[u];
                res+=w;
            }
        }
        int components=0;
        for(int i=0; i<n; i++){
            if(find(i)==i)
                components++;
        }
        if(components>1) // MST couldn't be formed, a bridge edge might have excluded
            return 1e9;
        return res;
    }
    vector<vector<int>> findCriticalAndPseudoCriticalEdges(int n, vector<vector<int>>& edges) {
        for(int i=0; i<edges.size(); i++)   
            edges[i].push_back(i); // to preserve edge-id after sorting
        sort(edges.begin(), edges.end(), [=](const vector<int>& v1, const vector<int>& v2){
           return v1[2] < v2[2];  // sort wrt weight
        });
        int cost = MST(n, edges);
        vector<int> critical, pCritical;
        for(int i=0; i<edges.size(); i++){
            if(MST(n, edges, -1, i) > cost)critical.push_back(edges[i][3]);
            else if(MST(n, edges, i, -1)==cost)pCritical.push_back(edges[i][3]);
        }
        return {critical, pCritical};
    }
};
```