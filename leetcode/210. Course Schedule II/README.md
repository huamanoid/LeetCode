# [210. Course Schedule II (Medium)](https://leetcode.com/problems/course-schedule-ii/)

<p>There are a total of <code>numCourses</code> courses you have to take, labeled from <code>0</code> to <code>numCourses - 1</code>. You are given an array <code>prerequisites</code> where <code>prerequisites[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> indicates that you <strong>must</strong> take course <code>b<sub>i</sub></code> first if you want to take course <code>a<sub>i</sub></code>.</p>

<ul>
	<li>For example, the pair <code>[0, 1]</code>, indicates that to take course <code>0</code> you have to first take course <code>1</code>.</li>
</ul>

<p>Return <em>the ordering of courses you should take to finish all courses</em>. If there are many valid answers, return <strong>any</strong> of them. If it is impossible to finish all courses, return <strong>an empty array</strong>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> numCourses = 2, prerequisites = [[1,0]]
<strong>Output:</strong> [0,1]
<strong>Explanation:</strong> There are a total of 2 courses to take. To take course 1 you should have finished course 0. So the correct course order is [0,1].
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> numCourses = 4, prerequisites = [[1,0],[2,0],[3,1],[3,2]]
<strong>Output:</strong> [0,2,1,3]
<strong>Explanation:</strong> There are a total of 4 courses to take. To take course 3 you should have finished both courses 1 and 2. Both courses 1 and 2 should be taken after you finished course 0.
So one correct course order is [0,1,2,3]. Another correct ordering is [0,2,1,3].
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> numCourses = 1, prerequisites = []
<strong>Output:</strong> [0]
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= numCourses &lt;= 2000</code></li>
	<li><code>0 &lt;= prerequisites.length &lt;= numCourses * (numCourses - 1)</code></li>
	<li><code>prerequisites[i].length == 2</code></li>
	<li><code>0 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt; numCourses</code></li>
	<li><code>a<sub>i</sub> != b<sub>i</sub></code></li>
	<li>All the pairs <code>[a<sub>i</sub>, b<sub>i</sub>]</code> are <strong>distinct</strong>.</li>
</ul>


**Related Topics**:  
[Depth-First Search](https://leetcode.com/tag/depth-first-search/), [Breadth-First Search](https://leetcode.com/tag/breadth-first-search/), [Graph](https://leetcode.com/tag/graph/), [Topological Sort](https://leetcode.com/tag/topological-sort/)

**Similar Questions**:
* [Course Schedule (Medium)](https://leetcode.com/problems/course-schedule/)
* [Alien Dictionary (Hard)](https://leetcode.com/problems/alien-dictionary/)
* [Minimum Height Trees (Medium)](https://leetcode.com/problems/minimum-height-trees/)
* [Sequence Reconstruction (Medium)](https://leetcode.com/problems/sequence-reconstruction/)
* [Course Schedule III (Hard)](https://leetcode.com/problems/course-schedule-iii/)
* [Parallel Courses (Medium)](https://leetcode.com/problems/parallel-courses/)

## Solution 1. Topological Sorting - Indegree method (Kahn's Algorithm)

```cpp
// OJ: https://leetcode.com/problems/course-schedule-ii/
// Author: A M A N
// Time : O(V + E)
// Space: O(V)
class Solution {
public:
    vector<int> findOrder(int n, vector<vector<int>>& prerequisites) {
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
        int cnt=0; // number of courses taken
        vector<int>topological;
        while(q.size()){
            int u = q.front(); q.pop();
            topological.push_back(u);
            for(auto v: adj[u]){
                inDegree[v]--;
                if(inDegree[v]==0)
                    q.push(v);
            }
            cnt++;
        }// at the end all the nodes inDegree must be zero if the DAG doesn't contain a cycle
        if(cnt==n){
            reverse(topological.begin(), topological.end());
            return topological;
        }
        return {};
    }
};
```


## Solution 2. DFS - Topological Sorting

```cpp
// OJ: https://leetcode.com/problems/course-schedule-ii/
// Author: A M A N
// Time : O(V + E)
// Space: O(V)
class Solution {
public:
    vector<int> ans;
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
        ans.push_back(u);
        return true;
    }
    vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
        vector<vector<int>> adj(numCourses);
        for(auto v: prerequisites)
            adj[v[0]].push_back(v[1]);
        int n = numCourses;
        vector<bool> vis(n, false);
        vector<bool> act(n, false);
        for(int i=0; i<n; i++){
            if(!vis[i]){
                if(!dfs(adj, vis, act, i)){
                    return {};            
                }
            }
        }
        return ans;
    }
};
```