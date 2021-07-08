# [743. Network Delay Time (Medium)](https://leetcode.com/problems/network-delay-time/)

<p>You are given a network of <code>n</code> nodes, labeled from <code>1</code> to <code>n</code>. You are also given <code>times</code>, a list of travel times as directed edges <code>times[i] = (u<sub>i</sub>, v<sub>i</sub>, w<sub>i</sub>)</code>, where <code>u<sub>i</sub></code> is the source node, <code>v<sub>i</sub></code> is the target node, and <code>w<sub>i</sub></code> is the time it takes for a signal to travel from source to target.</p>

<p>We will send a signal from a given node <code>k</code>. Return the time it takes for all the <code>n</code> nodes to receive the signal. If it is impossible for all the <code>n</code> nodes to receive the signal, return <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2019/05/23/931_example_1.png" style="width: 217px; height: 239px;">
<pre><strong>Input:</strong> times = [[2,1,1],[2,3,1],[3,4,1]], n = 4, k = 2
<strong>Output:</strong> 2
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> times = [[1,2,1]], n = 2, k = 1
<strong>Output:</strong> 1
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> times = [[1,2,1]], n = 2, k = 2
<strong>Output:</strong> -1
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= n &lt;= 100</code></li>
	<li><code>1 &lt;= times.length &lt;= 6000</code></li>
	<li><code>times[i].length == 3</code></li>
	<li><code>1 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt;= n</code></li>
	<li><code>u<sub>i</sub> != v<sub>i</sub></code></li>
	<li><code>0 &lt;= w<sub>i</sub> &lt;= 100</code></li>
	<li>All the pairs <code>(u<sub>i</sub>, v<sub>i</sub>)</code> are <strong>unique</strong>. (i.e., no multiple edges.)</li>
</ul>


**Related Topics**:  
[Depth-First Search](https://leetcode.com/tag/depth-first-search/), [Breadth-First Search](https://leetcode.com/tag/breadth-first-search/), [Graph](https://leetcode.com/tag/graph/), [Heap (Priority Queue)](https://leetcode.com/tag/heap-priority-queue/), [Shortest Path](https://leetcode.com/tag/shortest-path/)

## Solution 1. SPFA (Shortest Path Fast Algorithm) - BFS

```cpp
// OJ: https://leetcode.com/problems/network-delay-time/
// Author: A M A N
// Time average: O(E)
// Time worst  : O(V * E) 
// Space: O(V + E)
class Solution {
public:
    int networkDelayTime(vector<vector<int>>& times, int n, int k) {
        vector<vector<pair<int, int>>> adj(n);
        for(auto v: times)
            adj[v[0]-1].push_back({v[1]-1, v[2]});
        k--;
        vector<int> d(n, 1e9);
        d[k] = 0;
        queue<int>q;
        q.push(k);
        while(!q.empty()){
            int u = q.front(); q.pop();
            for(auto [v, w] : adj[u]){
                if(d[v] > d[u] + w){
                    d[v] = d[u] + w;
                    q.push(v);
                }
            }
        }
        int delay = *max_element(d.begin(), d.end());
        return delay==1e9?-1:delay;
    }
};
```


## Solution 2. Bellman-Ford Algorithm

```cpp
// OJ: https://leetcode.com/problems/network-delay-time/
// Author: A M A N
// Time : O(V * E)
// Space: O(V)
class Solution {
public:
       int networkDelayTime(vector<vector<int>>&times, int n, int k){
        vector<int> d(n, 1e9);
        d[--k]=0;
        for(int i=0; i<n; i++){
            for(auto e: times){
                int u = e[0], v = e[1], w = e[2];
                u--,v--;
                d[v] = min(d[v], d[u]+w);
            }
        }
        int delay = *max_element(d.begin(), d.end());
        return delay==1e9?-1:delay;
    }
};
```

## Solution 3. Floyd-Warshall Algorithm

```cpp
// OJ: https://leetcode.com/problems/network-delay-time/
// Author: A M A N
// Time : O(V^3)
// Space: O(V^2)
class Solution {
public:
        int networkDelayTime(vector<vector<int>>&times, int n, int z){
            vector<vector<int>> d(n,vector<int>(n, 1e9));
            for(auto v: times)
                d[v[0]-1][v[1]-1] = v[2];
            
            for(int i=0; i<n; i++)
                d[i][i]=0;
            
            for(int k=0; k<n; k++)
                for(int i=0; i<n; i++)
                    for(int j=0; j<n; j++)
                        d[i][j] = min(d[i][j], d[i][k]+d[k][j]);
            
            int delay = *max_element(d[z-1].begin(), d[z-1].end());
            return delay==1e9?-1:delay;
        }
};
```

## Solution 4. Dijkstra's Algorithm

```cpp
// OJ: https://leetcode.com/problems/network-delay-time/
// Author: A M A N
// Time : O(E + V*logV) almost linear
// Space: O(E + V)
class Solution {
public:
    int networkDelayTime(vector<vector<int>>& times, int n, int k) {
        vector<vector<pair<int, int>>>adj(n);
        for(auto v: times)
            adj[v[0]-1].push_back({v[1]-1, v[2]});
        vector<int>d(n, 1e9);
        vector<bool>vis(n, false);
        priority_queue<pair<int, int>> pq; // default pq is max pq
        k--;
        pq.push({0, k});
        // vis[k] = 1;              // this is a wrong version
        d[k]   = 0;
        while(!pq.empty()){
            int u = pq.top().second; pq.pop();
            if(vis[u])continue;
            vis[u] = 1;
            for(auto [v, w]: adj[u]){
                // if(!vis[v])      // this is a wrong version
                if(d[v] > d[u] + w){ 
                    d[v] = d[u] + w;
                    // vis[v] = 1;  // this is a wrong version 
                    pq.push({-d[v] , v}); 
                }
            }
        }
        int delay = *max_element(d.begin(), d.end());
        return delay==1e9?-1: delay;
    }
};
```