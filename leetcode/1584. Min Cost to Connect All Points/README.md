# [1584. Min Cost to Connect All Points (Medium)](https://leetcode.com/problems/min-cost-to-connect-all-points/)

<p>You are given an array&nbsp;<code>points</code>&nbsp;representing integer coordinates of some points on a 2D-plane, where <code>points[i] = [x<sub>i</sub>, y<sub>i</sub>]</code>.</p>

<p>The cost of connecting two points <code>[x<sub>i</sub>, y<sub>i</sub>]</code> and <code>[x<sub>j</sub>, y<sub>j</sub>]</code> is the <strong>manhattan distance</strong> between them:&nbsp;<code>|x<sub>i</sub> - x<sub>j</sub>| + |y<sub>i</sub> - y<sub>j</sub>|</code>, where <code>|val|</code> denotes the absolute value of&nbsp;<code>val</code>.</p>

<p>Return&nbsp;<em>the minimum cost to make all points connected.</em> All points are connected if there is <strong>exactly one</strong> simple path between any two points.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<p><img alt="" src="https://assets.leetcode.com/uploads/2020/08/26/d.png" style="width: 214px; height: 268px;"></p>

<pre><strong>Input:</strong> points = [[0,0],[2,2],[3,10],[5,2],[7,0]]
<strong>Output:</strong> 20
<strong>Explanation:
</strong><img alt="" src="https://assets.leetcode.com/uploads/2020/08/26/c.png" style="width: 214px; height: 268px;">
We can connect the points as shown above to get the minimum cost of 20.
Notice that there is a unique path between every pair of points.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> points = [[3,12],[-2,5],[-4,1]]
<strong>Output:</strong> 18
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> points = [[0,0],[1,1],[1,0],[-1,1]]
<strong>Output:</strong> 4
</pre>

<p><strong>Example 4:</strong></p>

<pre><strong>Input:</strong> points = [[-1000000,-1000000],[1000000,1000000]]
<strong>Output:</strong> 4000000
</pre>

<p><strong>Example 5:</strong></p>

<pre><strong>Input:</strong> points = [[0,0]]
<strong>Output:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= points.length &lt;= 1000</code></li>
	<li><code>-10<sup>6</sup>&nbsp;&lt;= x<sub>i</sub>, y<sub>i</sub> &lt;= 10<sup>6</sup></code></li>
	<li>All pairs <code>(x<sub>i</sub>, y<sub>i</sub>)</code> are distinct.</li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Union Find](https://leetcode.com/tag/union-find/), [Minimum Spanning Tree](https://leetcode.com/tag/minimum-spanning-tree/)

## Solution 1. Kruskal's Algorithm (Greedy)

1. Sort the edges in increasing order
2. Choose the edge if it doesn't form cycle.

```cpp
// OJ: https://leetcode.com/problems/min-cost-to-connect-all-points/
// Author: A M A N
// Time : O(N^2 * log N^2)
// Space: O(N^2)
class Solution {
public:
    
    unordered_map<int, int> p;
    unordered_map<int, int> sz;
    
    int find(int u){
        if(p.find(u)==p.end())
            p[u] = u;
        while(p[u]!=u){
            p[u] = p[p[u]]; // path compression
            u = p[u];
        }
        return u;
    }
    
    int minCostConnectPoints(vector<vector<int>>& points) {
        vector<pair<int, pair<int, int>>> edges;
        for(int i=0; i<points.size(); i++){
            for(int j=i+1; j<points.size(); j++){
                int w = abs(points[i][0] - points[j][0]) + abs(points[i][1]-points[j][1]);
                edges.push_back({w, {i, j}});
            }
        }
        
        sort(edges.begin(), edges.end());
        int ans = 0;
        for(auto& e : edges){
            int w  = e.first;
            auto [u, v] = e.second;
            
            u = find(u);
            v = find(v);
            
            if(u!=v){  // select the current edge if doesn't form cycle 
                //union by weight
                if(sz[u]>sz[v]) p[v] = u, sz[u]+=sz[v]; 
                else            p[u] = v, sz[v]+=sz[u]; 
                ans+=w;
            }
        }
        return ans;
    }
};
```


`Heap` has an advantage if we are pulling m < n elements. If we are pulling all n elements - no difference compared to a sort. This is because the heap is O(m log n), while the sort is O(n log n).

```cpp
// OJ: https://leetcode.com/problems/min-cost-to-connect-all-points/
// Author: A M A N
// Time : O(K * log N^2) where K is the number of edges we need to pull to complete the tree. It's much smaller than n * n in the average case.
// Space: O(N^2)
class Solution {
public:
    
    unordered_map<int, int> p;
    unordered_map<int, int> sz;
    
    int find(int u){
        if(p.find(u)==p.end())
            p[u] = u;
        while(p[u]!=u){
            p[u] = p[p[u]]; // path compression
            u = p[u];
        }
        return u;
    }
    
    int minCostConnectPoints(vector<vector<int>>& points) {
        int n = points.size();
        vector<array<int, 3>> edges;
        for(int i=0; i<points.size(); i++){
            for(int j=i+1; j<points.size(); j++){
                int w = abs(points[i][0] - points[j][0]) + abs(points[i][1]-points[j][1]);
                edges.push_back({w, i, j});
            }
        }
        
        // Building a heap from the array is O(n), then pulling one element is O(log n). That's why using make_heap instead of adding elements to the heap one-by-one.
        make_heap(edges.begin(), edges.end(), greater<array<int,3>>()); 

        int cnt=0; 
        int ans = 0;
        while(!edges.empty()){
            pop_heap(edges.begin(), edges.end(), greater<array<int,3>>()); // takes the top element in the heap and moves it to the back of the array, moves the largest to the end
            auto [w, u, v] = edges.back();
            edges.pop_back();
            
            u = find(u);
            v = find(v);
            
            if(u!=v){  // select the current edge if doesn't form cycle 
                cnt++; // edges in MST;
                if(sz[u]>sz[v]) p[v] = u, sz[u]+=sz[v];                 //union by weight
                else            p[u] = v, sz[v]+=sz[u]; 
                ans+=w;
            }
            
            if(cnt==n-1)break; // MST have been formed
        }
        return ans;
    }
};
```


## Solution 2. Prim's Algorithm

1. Start from a random node (here it's `0`)
2. Add a new neighbour node with minimum distance from `any` visited node in the MST
3. Repeat Step 2 until all the nodes are added to MST (or edges in MST == N-1)

```cpp
// OJ: https://leetcode.com/problems/min-cost-to-connect-all-points/
// Author: A M A N
// Time : O(N^2 * log N)
// Space: O(N)
class Solution {
public:
    int minCostConnectPoints(vector<vector<int>>& points) {
        priority_queue<pair<int, int>> pq;
        int cur = 0, ans=0, n = points.size();
        vector<bool>vis(n, false);
        for(int i=0; i < n-1; i++){ // N-1 edges in MST
            vis[cur] = true;
            for(int i=0; i<n; i++)
                if(!vis[i]){
                    int d = abs(points[cur][0]-points[i][0]) + abs(points[cur][1] - points[i][1]);
                    pq.push({-d, i}); // pushing only neighbour nodes to the heap
                }
            
            while(vis[pq.top().second])
                pq.pop();
            ans -= pq.top().first;
            cur  = pq.top().second; 
            pq.pop();
        }
        return ans;
    }
};

```
A little faster version by maintaining a distance array.
Note that the next node to be added might not be adjacent to the cur node,
it can be adjacent to any of the visited node. So, `it's not a BFS`

```cpp
// OJ: https://leetcode.com/problems/min-cost-to-connect-all-points/
// Author: A M A N
// Time : O(N^2)
// Space: O(N)
class Solution {
public:
    int minCostConnectPoints(vector<vector<int>>& points) {
        int cur = 0, ans=0, n = points.size();
        vector<int> vis(n, false), d(n, INT_MAX);
        for(int i=0; i < n-1; i++){
            vis[cur] = true;
            for(int i=0; i<n; i++)
                if(!vis[i])
                    d[i] = min(d[i], abs(points[cur][0]-points[i][0]) + abs(points[cur][1] - points[i][1]));

            cur = min_element(d.begin(), d.end()) - d.begin(); // not necessarily a node connected directly to the cur node, might be the neighbour of an earlier visited node, and distance to reach the new node is minimum from an earlier visited node than from the cur node. 
            // suppose the MST till now comprises of {1, 4, 5} and the cur node is 5. now to reach 2 the cost from cur node (5) is 3 but to reach the same node 2 the cost from earlier visited node is say 1.
            // d[2] = min(d[2], 3) when d[2] = 1; Got it? 
            ans += d[cur];
            d[cur] = INT_MAX;
        }
        return ans;
    }
};




```