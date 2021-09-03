# [1168. Optimize Water Distribution in a Village (Hard)](https://leetcode.com/problems/optimize-water-distribution-in-a-village/)

<p>There are <code>n</code> houses in a village. We want to supply water for all the houses by building wells and laying pipes.</p>

<p>For each house <code>i</code>, we can either build a well inside it directly with cost <code>wells[i - 1]</code> (note the <code>-1</code> due to <strong>0-indexing</strong>), or pipe in water from another well to it. The costs to lay pipes between houses are given by the array <code>pipes</code>, where each <code>pipes[j] = [house1<sub>j</sub>, house2<sub>j</sub>, cost<sub>j</sub>]</code> represents the cost to connect <code>house1<sub>j</sub></code> and <code>house2<sub>j</sub></code> together using a pipe. Connections are bidirectional.</p>

<p>Return <em>the minimum total cost to supply water to all houses</em>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2019/05/22/1359_ex1.png" style="width: 189px; height: 196px;">
<pre><strong>Input:</strong> n = 3, wells = [1,2,2], pipes = [[1,2,1],[2,3,1]]
<strong>Output:</strong> 3
<strong>Explanation: </strong>
The image shows the costs of connecting houses using pipes.
The best strategy is to build a well in the first house with cost 1 and connect the other houses to it with cost 2 so the total cost is 3.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> n = 2, wells = [1,1], pipes = [[1,2,1]]
<strong>Output:</strong> 2
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 10<sup>4</sup></code></li>
	<li><code>wells.length == n</code></li>
	<li><code>0 &lt;= wells[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= pipes.length &lt;= 10<sup>4</sup></code></li>
	<li><code>pipes[j].length == 3</code></li>
	<li><code>1 &lt;= house1<sub>j</sub>, house2<sub>j</sub> &lt;= n</code></li>
	<li><code>0 &lt;= cost<sub>j</sub> &lt;= 10<sup>5</sup></code></li>
	<li><code>house1<sub>j</sub> != house2<sub>j</sub></code></li>
</ul>


**Companies**:  
[Google](https://leetcode.com/company/google), [Facebook](https://leetcode.com/company/facebook), [Yahoo](https://leetcode.com/company/yahoo)

**Related Topics**:  
[Union Find](https://leetcode.com/tag/union-find/), [Graph](https://leetcode.com/tag/graph/), [Minimum Spanning Tree](https://leetcode.com/tag/minimum-spanning-tree/)

## Solution 1. Kruskal's Algorithm (MST)

```cpp
// OJ: https://leetcode.com/problems/optimize-water-distribution-in-a-village/
// Author: A M A N
// Time : O((N + M)*log(N + M)) where N be the number of houses (wells), and M be the number of pipes
// Space: O(N + M)
class Solution {
    int p[100002], sz[100002];
    int find(int u){
        while(p[u]!=u){
            p[u] = p[p[u]]; //path compression
            u = p[u];
        }
        return u;
    }
public:
    int minCostToSupplyWater(int n, vector<int>& wells, vector<vector<int>>& pipes) {
        vector<vector<int>> edges;
        //handling wells
        for(int i=0; i<wells.size(); i++) // 0th node is the virtual node for wells with edges connecting all houses
            edges.push_back({0, i+1, wells[i]}); 
        //handling pipes
        for(auto p: pipes)
            edges.push_back(p);
        sort(edges.begin(), edges.end(), [&](const vector<int>& a, const vector<int>& b){return a[2]<b[2];}); // sorting wrt cost
        iota(p, p+n+1, 0); 
        fill(sz, sz+n+1, 1);
        int ans = 0;
        for(auto e: edges){
            int u = find(e[0]), v = find(e[1]), w = e[2];
            if(u!=v){
                if(sz[u]>sz[v]){
                    p[v] = u;
                    sz[u]+=sz[v];     //union by size               
                }else{
                    p[u] = v;
                    sz[v]+=sz[u];
                }
                ans += w;
            }
        }
        return ans;
    }
};
```