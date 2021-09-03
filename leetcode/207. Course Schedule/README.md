# [207. Course Schedule (Medium)](https://leetcode.com/problems/course-schedule/)

<p>There are a total of <code>numCourses</code> courses you have to take, labeled from <code>0</code> to <code>numCourses - 1</code>. You are given an array <code>prerequisites</code> where <code>prerequisites[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> indicates that you <strong>must</strong> take course <code>b<sub>i</sub></code> first if you want to take course <code>a<sub>i</sub></code>.</p>

<ul>
	<li>For example, the pair <code>[0, 1]</code>, indicates that to take course <code>0</code> you have to first take course <code>1</code>.</li>
</ul>

<p>Return <code>true</code> if you can finish all courses. Otherwise, return <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> numCourses = 2, prerequisites = [[1,0]]
<strong>Output:</strong> true
<strong>Explanation:</strong> There are a total of 2 courses to take. 
To take course 1 you should have finished course 0. So it is possible.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> numCourses = 2, prerequisites = [[1,0],[0,1]]
<strong>Output:</strong> false
<strong>Explanation:</strong> There are a total of 2 courses to take. 
To take course 1 you should have finished course 0, and to take course 0 you should also have finished course 1. So it is impossible.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= numCourses &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= prerequisites.length &lt;= 5000</code></li>
	<li><code>prerequisites[i].length == 2</code></li>
	<li><code>0 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt; numCourses</code></li>
	<li>All the pairs prerequisites[i] are <strong>unique</strong>.</li>
</ul>


**Related Topics**:  
[Depth-First Search](https://leetcode.com/tag/depth-first-search/), [Breadth-First Search](https://leetcode.com/tag/breadth-first-search/), [Graph](https://leetcode.com/tag/graph/), [Topological Sort](https://leetcode.com/tag/topological-sort/)

**Similar Questions**:
* [Course Schedule II (Medium)](https://leetcode.com/problems/course-schedule-ii/)
* [Graph Valid Tree (Medium)](https://leetcode.com/problems/graph-valid-tree/)
* [Minimum Height Trees (Medium)](https://leetcode.com/problems/minimum-height-trees/)
* [Course Schedule III (Hard)](https://leetcode.com/problems/course-schedule-iii/)

## Solution 1. DFS - Topological Sorting

```cpp
// OJ: https://leetcode.com/problems/course-schedule/
// Author: A M A N
// Time : O(V + E)
// Space: O(V)
class Solution {
public:
    //Topological Sorting
    bool dfs(vector<vector<int>>&adj, vector<bool> &vis, vector<bool>&act, int u){
        vis[u] = 1;
        act[u] = 1;
        for(auto v: adj[u]){
            if(act[v])return false;
            if(vis[v])continue;
            if(!dfs(adj, vis, act, v))
                return false;
        }
        act[u] = 0;
        return true;
    }
    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
        vector<vector<int>> adj(numCourses);
        for(auto v: prerequisites)
            adj[v[0]].push_back(v[1]);
        int n = numCourses;
        vector<bool> vis(n, false);
        vector<bool> act(n, false);
        for(int i=0; i<n; i++){
            if(!vis[i]){
                if(!dfs(adj, vis, act, i)){
                    return false;            
                }
            }
        }
        return true;
    }
};
```

## Solution 2. Indegree method (Kahn's Algorithm)

```cpp
// OJ: https://leetcode.com/problems/course-schedule/
// Author: A M A N
// Time : O(V + E)
// Space: O(V)
class Solution {
public:
    bool canFinish(int n, vector<vector<int>>& prerequisites) {
        vector<vector<int>> adj(n);
        vector<int>inDegree(n, 0);
        for(auto edge: prerequisites){
            inDegree[edge[1]]++;
            adj[edge[0]].push_back(edge[1]);
        }
        queue<int> q;
        for(int i=0; i<n; i++)
            if(inDegree[i]==0)
                q.push(i);
        //the idea is topmost node in the topological sort has no incoming edges
        int cnt=0; //number of courses taken
        while(q.size()){
            int u = q.front(); q.pop();
            for(auto v: adj[u]){
                inDegree[v]--;
                if(inDegree[v]==0)
                    q.push(v);
            }
            cnt++;
        }// at the end all the nodes inDegree must be zero if the DAG doesn't contain a cycle
        return (cnt==n);
    }
};
```