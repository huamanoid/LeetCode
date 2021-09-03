# [787. Cheapest Flights Within K Stops (Medium)](https://leetcode.com/problems/cheapest-flights-within-k-stops/)

<p>There are <code>n</code> cities connected by some number of flights. You are given an array <code>flights</code> where <code>flights[i] = [from<sub>i</sub>, to<sub>i</sub>, price<sub>i</sub>]</code> indicates that there is a flight from city <code>from<sub>i</sub></code> to city <code>to<sub>i</sub></code> with cost <code>price<sub>i</sub></code>.</p>

<p>You are also given three integers <code>src</code>, <code>dst</code>, and <code>k</code>, return <em><strong>the cheapest price</strong> from </em><code>src</code><em> to </em><code>dst</code><em> with at most </em><code>k</code><em> stops. </em>If there is no such route, return<em> </em><code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://s3-lc-upload.s3.amazonaws.com/uploads/2018/02/16/995.png" style="height: 360px; width: 492px;">
<pre><strong>Input:</strong> n = 3, flights = [[0,1,100],[1,2,100],[0,2,500]], src = 0, dst = 2, k = 1
<strong>Output:</strong> 200
<strong>Explanation:</strong> The graph is shown.
The cheapest price from city <code>0</code> to city <code>2</code> with at most 1 stop costs 200, as marked red in the picture.
</pre>

<p><strong>Example 2:</strong></p>
<img alt="" src="https://s3-lc-upload.s3.amazonaws.com/uploads/2018/02/16/995.png" style="height: 360px; width: 492px;">
<pre><strong>Input:</strong> n = 3, flights = [[0,1,100],[1,2,100],[0,2,500]], src = 0, dst = 2, k = 0
<strong>Output:</strong> 500
<strong>Explanation:</strong> The graph is shown.
The cheapest price from city <code>0</code> to city <code>2</code> with at most 0 stop costs 500, as marked blue in the picture.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 100</code></li>
	<li><code>0 &lt;= flights.length &lt;= (n * (n - 1) / 2)</code></li>
	<li><code>flights[i].length == 3</code></li>
	<li><code>0 &lt;= from<sub>i</sub>, to<sub>i</sub> &lt; n</code></li>
	<li><code>from<sub>i</sub> != to<sub>i</sub></code></li>
	<li><code>1 &lt;= price<sub>i</sub> &lt;= 10<sup>4</sup></code></li>
	<li>There will not be any multiple flights between two cities.</li>
	<li><code>0 &lt;= src, dst, k &lt; n</code></li>
	<li><code>src != dst</code></li>
</ul>


**Related Topics**:  
[Dynamic Programming](https://leetcode.com/tag/dynamic-programming/), [Depth-First Search](https://leetcode.com/tag/depth-first-search/), [Breadth-First Search](https://leetcode.com/tag/breadth-first-search/), [Graph](https://leetcode.com/tag/graph/), [Heap (Priority Queue)](https://leetcode.com/tag/heap-priority-queue/), [Shortest Path](https://leetcode.com/tag/shortest-path/)

**Similar Questions**:
* [Maximum Vacation Days (Hard)](https://leetcode.com/problems/maximum-vacation-days/)

## Solution 1. Bellman-Ford Algorithm

The `i`'th iteration in Bellman-Ford `doesn't` minimize the distance with always `i edges`. In one iteration it can relax the distance of new node with the help of an already relaxed node in the same loop.

<code> dist[v] = min(dist[v], dist[u] + w)</code>
>so BF Algo relaxes a node using more than i edges at the i'th iteration. 

we tackle this by creating a copy of the distance array at each iteration and relaxing the copy with the help of original distance array.
This makes sure we're not taking help of an already relaxed node in the same iteration.
And after each iteration we swap the swap the copy and original.

<code> temp[v] = min(temp[u], dist[u] + w)</code>

>Now at i'th iteration the distance b/w any two nodes will have at max i edges.
```cpp
// OJ: https://leetcode.com/problems/cheapest-flights-within-k-stops/
// Author: A M A N
// Time : O(K * E)
// Space: O(V)
class Solution {
public:
    // Bellman-Ford Algorithm
    int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int K) {
        vector<int>d(n, 1e9);
        d[src]=0;
        for(int i=0; i<=K; i++){ // K stops => max K+1 edges
            vector<int> temp(d); 
            for(auto edge: flights){
                int u = edge[0], v = edge[1], w = edge[2];
                temp[v] = min(temp[v], d[u]+w); // using original distances to relax the edges, 
                //not using the previously relaxed edge in the same loop. i.e. relaxing with the help of one edge in one loop
            }
            d = temp;
        }
        return d[dst]>=1e9?-1:d[dst];
    }
};
```

## Solution 2. DFS + Memoization

```cpp
// OJ: https://leetcode.com/problems/cheapest-flights-within-k-stops/
// Author: A M A N
// Time : O(N^2)
// Space: O(N^2)
class Solution {
    static const int N = 101;
    int dp[N][N];
public:
    int solve(vector<vector<pair<int, int>>> &adj, int start, int end, int k){
        // Base Cases
        if(start==end)
            return 0;
        if(k==-1) // we can take atmost k+1 flights
            return 1e9;
        if(dp[start][k]!=-1)
            return dp[start][k];
        int ans = 1e9;        
        for(auto [next, cost]: adj[start]){
            ans = min(ans, cost + solve(adj, next, end, k-1));
        }
        return dp[start][k] = ans;
    }
    int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int k) {
        memset(dp, -1, sizeof dp);       
        vector<vector<pair<int, int>>>adj(n);
        for(auto f: flights)
            adj[f[0]].push_back({f[1], f[2]});
        int minCost = solve(adj, src, dst, k);
        return minCost>=1e9?-1: minCost;
    }
};
```