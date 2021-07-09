# [886. Possible Bipartition (Medium)](https://leetcode.com/problems/possible-bipartition/)

<p>Given a set of <code>n</code>&nbsp;people (numbered <code>1, 2, ..., n</code>), we would like to split everyone into two groups of <strong>any</strong> size.</p>

<p>Each person may dislike some other people, and they should not go into the same group.&nbsp;</p>

<p>Formally, if <code>dislikes[i] = [a, b]</code>, it means it is not allowed to put the people numbered <code>a</code> and <code>b</code> into the same group.</p>

<p>Return <code>true</code>&nbsp;if and only if it is possible to split everyone into two groups in this way.</p>

<p>&nbsp;</p>

<div>
<div>
<ol>
</ol>
</div>
</div>

<div>
<p><strong>Example 1:</strong></p>

<pre><strong>Input: </strong>n = <span id="example-input-1-1">4</span>, dislikes = <span id="example-input-1-2">[[1,2],[1,3],[2,4]]</span>
<strong>Output: </strong><span id="example-output-1">true</span>
<strong>Explanation</strong>: group1 [1,4], group2 [2,3]
</pre>

<div>
<p><strong>Example 2:</strong></p>

<pre><strong>Input: </strong>n = <span id="example-input-2-1">3</span>, dislikes = <span id="example-input-2-2">[[1,2],[1,3],[2,3]]</span>
<strong>Output: </strong><span id="example-output-2">false</span>
</pre>

<div>
<p><strong>Example 3:</strong></p>

<pre><strong>Input: </strong>n = <span id="example-input-3-1">5</span>, dislikes = <span id="example-input-3-2">[[1,2],[2,3],[3,4],[4,5],[1,5]]</span>
<strong>Output: </strong><span id="example-output-3">false</span>
</pre>
</div>
</div>
</div>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 2000</code></li>
	<li><code>0 &lt;= dislikes.length &lt;= 10000</code></li>
	<li><code>dislikes[i].length == 2</code></li>
	<li><code>1 &lt;= dislikes[i][j] &lt;= n</code></li>
	<li><code>dislikes[i][0] &lt; dislikes[i][1]</code></li>
	<li>There does not exist <code>i != j</code> for which <code>dislikes[i] == dislikes[j]</code>.</li>
</ul>


**Related Topics**:  
[Depth-First Search](https://leetcode.com/tag/depth-first-search/), [Breadth-First Search](https://leetcode.com/tag/breadth-first-search/), [Union Find](https://leetcode.com/tag/union-find/), [Graph](https://leetcode.com/tag/graph/)

## Solution 1. BFS - Bipartite

```cpp
// OJ: https://leetcode.com/problems/possible-bipartition/
// Author: A M A N
// Time : O(V + E)
// Space: O(V + E)
class Solution {
public:
    bool bip(vector<vector<int>> & adj, int i, vector<int>&col){
        queue<int> q;
        q.push(i);
        col[0] = 0;
        while(!q.empty()){
            int u = q.front(); q.pop();
            for(auto v: adj[u]){
                if(col[v]==-1){
                    col[v] = col[u]^1;
                    q.push(v);
                }else if(col[v]==col[u]){
                    return false;
                }
            }
        }
        return true;
    }
    bool possibleBipartition(int n, vector<vector<int>>& dislikes){
        vector<vector<int>> adj(n);
        for(auto v: dislikes){
            adj[v[0]-1].push_back(v[1]-1);
            adj[v[1]-1].push_back(v[0]-1);
        }
        vector<int> col(n, -1);
        for(int i=0; i<n; i++){
            if(col[i]!=-1)continue;
            if(!bip(adj, i, col))
                return false;
        }
        return true;
    }
};
```


## Solution 2. DFS - Bipartite

```cpp
// OJ: https://leetcode.com/problems/possible-bipartition/
// Author: A M A N
// Time : O(V + E)
// Space: O(V + E)
class Solution {
public:
    bool bip(vector<vector<int>> & adj, int u, vector<int>&col){
        for(auto v: adj[u]){
            if(col[v]==-1){
                col[v] = col[u]^1;
                bip(adj, v, col);
            }else if(col[v]==col[u])
                return false;
        }
        return true;
    }
    bool possibleBipartition(int n, vector<vector<int>>& dislikes){
        vector<vector<int>> adj(n);
        for(auto v: dislikes){
            adj[v[0]-1].push_back(v[1]-1);
            adj[v[1]-1].push_back(v[0]-1);
        }
        vector<int> col(n, -1);
        for(int i=0; i<n; i++){
            if(col[i]!=-1)continue;
            col[i] = 0;
            if(!bip(adj, i, col))
                return false;
        }
        return true;
    }
};
```