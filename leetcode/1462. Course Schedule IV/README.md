# [1462. Course Schedule IV (Medium)](https://leetcode.com/problems/course-schedule-iv/)

<p>There are a total of <code>numCourses</code> courses you have to take, labeled from <code>0</code> to <code>numCourses - 1</code>. You are given an array <code>prerequisites</code> where <code>prerequisites[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> indicates that you <strong>must</strong> take course <code>b<sub>i</sub></code> first if you want to take course <code>a<sub>i</sub></code>.</p>

<ul>
	<li>For example, the pair <code>[0, 1]</code>, indicates that to take course <code>0</code> you have to first take course <code>1</code>.</li>
</ul>

<p>You are also given an array <code>queries</code> where <code>queries[j] = [u<sub>j</sub>, v<sub>j</sub>]</code>. For the <code>j<sup>th</sup></code> query, you should answer whether the course <code>u<sub>j</sub></code> is a prerequisite of the course <code>v<sub>j</sub></code> or not. Note that if course <code>a</code> is a prerequisite of course <code>b</code> and course <code>b</code> is a prerequisite of course <code>c</code>, then, course <code>a</code> is a prerequisite of course <code>c</code>.</p>

<p>Return <i>a boolean array </i><code>answer</code><i>, where </i><code>answer[j]</code><i> is the answer of the </i><code>j<sup>th</sup></code><i> query.</i></p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/05/01/courses4-1-graph.jpg" style="width: 222px; height: 62px;">
<pre><strong>Input:</strong> numCourses = 2, prerequisites = [[1,0]], queries = [[0,1],[1,0]]
<strong>Output:</strong> [false,true]
<strong>Explanation:</strong> course 0 is not a prerequisite of course 1 but the opposite is true.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> numCourses = 2, prerequisites = [], queries = [[1,0],[0,1]]
<strong>Output:</strong> [false,false]
<strong>Explanation:</strong> There are no prerequisites and each course is independent.
</pre>

<p><strong>Example 3:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/05/01/courses4-3-graph.jpg" style="width: 222px; height: 222px;">
<pre><strong>Input:</strong> numCourses = 3, prerequisites = [[1,2],[1,0],[2,0]], queries = [[1,0],[1,2]]
<strong>Output:</strong> [true,true]
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>2 &lt;= numCourses &lt;= 100</code></li>
	<li><code>0 &lt;= prerequisite.length &lt;= (numCourses * (numCourses - 1) / 2)</code></li>
	<li><code>0 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt; n</code></li>
	<li><code>a<sub>i</sub> != b<sub>i</sub></code></li>
	<li>All the pairs <code>[a<sub>i</sub>, b<sub>i</sub>]</code> are <strong>unique</strong>.</li>
	<li>The prerequisites graph has no cycles.</li>
	<li><code>1 &lt;= queries.length &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt; n</code></li>
	<li><code>u<sub>i</sub> != v<sub>i</sub></code></li>
</ul>


**Related Topics**:  
[Depth-First Search](https://leetcode.com/tag/depth-first-search/), [Breadth-First Search](https://leetcode.com/tag/breadth-first-search/), [Graph](https://leetcode.com/tag/graph/), [Topological Sort](https://leetcode.com/tag/topological-sort/)

## Solution 1. Floyd-Warshall Algorithm

```cpp
// OJ: https://leetcode.com/problems/course-schedule-iv/
// Author: A M A N
// Time : O(N^3)
// Space: O(N^2)
class Solution {
public:
    vector<bool> checkIfPrerequisite(int n, vector<vector<int>>& prerequisites, vector<vector<int>>& queries) {
        int g[n][n];
        memset(g, 0, sizeof g);
        for(auto q: prerequisites)
            g[q[0]][q[1]] = 1;
        //Floyd-Warshall Algorithm modified to check connectedness
        for(int k=0; k<n; k++)
            for(int i=0; i<n; i++)
                for(int j=0; j<n; j++)
                    g[i][j] |= g[i][k] and g[k][j];    
        vector<bool> ans;
        for(auto q: queries)
            ans.push_back(g[q[0]][q[1]]); //is connected or not
        return ans;
    }
};
```