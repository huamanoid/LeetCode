# [797. All Paths From Source to Target (Medium)](https://leetcode.com/problems/all-paths-from-source-to-target/)

<p>Given a directed&nbsp;acyclic graph (<strong>DAG</strong>) of <code>n</code> nodes labeled from 0 to n - 1,&nbsp;find all possible paths from node <code>0</code> to node <code>n - 1</code>, and return them in any order.</p>

<p>The graph is given as follows:&nbsp;<code>graph[i]</code> is a list of all nodes you can visit from node <code>i</code>&nbsp;(i.e., there is a directed edge from node <code>i</code> to node <code>graph[i][j]</code>).</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2020/09/28/all_1.jpg" style="width: 242px; height: 242px;">
<pre><strong>Input:</strong> graph = [[1,2],[3],[3],[]]
<strong>Output:</strong> [[0,1,3],[0,2,3]]
<strong>Explanation:</strong> There are two paths: 0 -&gt; 1 -&gt; 3 and 0 -&gt; 2 -&gt; 3.
</pre>

<p><strong>Example 2:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2020/09/28/all_2.jpg" style="width: 423px; height: 301px;">
<pre><strong>Input:</strong> graph = [[4,3,1],[3,2,4],[3],[4],[]]
<strong>Output:</strong> [[0,4],[0,3,4],[0,1,3,4],[0,1,2,3,4],[0,1,4]]
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> graph = [[1],[]]
<strong>Output:</strong> [[0,1]]
</pre>

<p><strong>Example 4:</strong></p>

<pre><strong>Input:</strong> graph = [[1,2,3],[2],[3],[]]
<strong>Output:</strong> [[0,1,2,3],[0,2,3],[0,3]]
</pre>

<p><strong>Example 5:</strong></p>

<pre><strong>Input:</strong> graph = [[1,3],[2],[3],[]]
<strong>Output:</strong> [[0,1,2,3],[0,3]]
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>n == graph.length</code></li>
	<li><code>2 &lt;= n &lt;= 15</code></li>
	<li><code>0 &lt;= graph[i][j] &lt; n</code></li>
	<li><code>graph[i][j] != i</code> (i.e., there will be no self-loops).</li>
	<li>The input graph is <strong>guaranteed</strong> to be a <strong>DAG</strong>.</li>
</ul>


**Related Topics**:  
[Backtracking](https://leetcode.com/tag/backtracking/), [Depth-First Search](https://leetcode.com/tag/depth-first-search/), [Breadth-First Search](https://leetcode.com/tag/breadth-first-search/), [Graph](https://leetcode.com/tag/graph/)

## Solution 1. Backtracking

```cpp
// OJ: https://leetcode.com/problems/all-paths-from-source-to-target/
// Author: A M A N
// Time : O(N * 2^N)
// Space: O(N)
class Solution {
public:
    vector<vector<int>>ans;
    void dfs(vector<vector<int>>& adj, int u, int end, vector<int> temp){
        if(u==end){
            ans.push_back(temp);
            return;
        }
        for(auto v: adj[u]){
            temp.push_back(v);
            dfs(adj, v, end, temp);
            temp.pop_back(); //backtrack
        }
    }
    vector<vector<int>> allPathsSourceTarget(vector<vector<int>>& adj) {
        vector<int> temp = {0};
        dfs(adj, 0, adj.size()-1, temp);
        return ans;
    }
};
```