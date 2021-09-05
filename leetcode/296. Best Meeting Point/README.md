# [296. Best Meeting Point (Hard)](https://leetcode.com/problems/best-meeting-point/)

<p>Given an <code>m x n</code> binary grid <code>grid</code> where each <code>1</code> marks the home of one friend, return <em>the minimal <strong>total travel distance</strong></em>.</p>

<p>The <strong>total travel distance</strong> is the sum of the distances between the houses of the friends and the meeting point.</p>

<p>The distance is calculated using <a href="http://en.wikipedia.org/wiki/Taxicab_geometry" target="_blank">Manhattan Distance</a>, where <code>distance(p1, p2) = |p2.x - p1.x| + |p2.y - p1.y|</code>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/03/14/meetingpoint-grid.jpg" style="width: 413px; height: 253px;">
<pre><strong>Input:</strong> grid = [[1,0,0,0,1],[0,0,0,0,0],[0,0,1,0,0]]
<strong>Output:</strong> 6
<strong>Explanation:</strong> Given three friends living at (0,0), (0,4), and (2,2).
The point (0,2) is an ideal meeting point, as the total travel distance of 2 + 2 + 2 = 6 is minimal.
So return 6.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> grid = [[1,1]]
<strong>Output:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 200</code></li>
	<li><code>grid[i][j]</code> is either <code>0</code> or <code>1</code>.</li>
	<li>There will be <strong>at least two</strong> friends in the <code>grid</code>.</li>
</ul>


**Companies**:  
[Facebook](https://leetcode.com/company/facebook)

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Math](https://leetcode.com/tag/math/), [Sorting](https://leetcode.com/tag/sorting/), [Matrix](https://leetcode.com/tag/matrix/)

**Similar Questions**:
* [Shortest Distance from All Buildings (Hard)](https://leetcode.com/problems/shortest-distance-from-all-buildings/)
* [Minimum Moves to Equal Array Elements II (Medium)](https://leetcode.com/problems/minimum-moves-to-equal-array-elements-ii/)

## Solution 1. Sorting

> Median minimzes total distance for absolute deviation

Since, the manhattan distance is independent of the other coordinate
we can break the problem in x and y dimensions and find the median to find out the meeting point.

```cpp
// OJ: https://leetcode.com/problems/best-meeting-point/
// Author: A M A N
// Time : O(m*n*log(m*n))
// Space: O(mn)
class Solution {
    int calculateDistance(vector<vector<int>>&grid, int x, int y){
        int distance = 0;
        for(int i=0; i<grid.size(); i++)
            for(int j=0; j<grid[0].size(); j++)
                if(grid[i][j])
                    distance+=abs(i-x) + abs(j-y);
        return distance;
    }
public:
    int minTotalDistance(vector<vector<int>>& grid) {
        vector<int> rows, cols;
        for(int i=0; i<grid.size(); i++)
            for(int j=0; j<grid[0].size(); j++)
                if(grid[i][j])
                    rows.push_back(i), cols.push_back(j);
        sort(rows.begin(), rows.end());
        sort(cols.begin(), cols.end());
        int medX = rows[rows.size()/2];
        int medY = cols[cols.size()/2];
        return calculateDistance(grid, medX, medY);
    }
};
```
we can solve without sorting.
by collecting both the row and column coordinates in sorted order

```cpp
// OJ: https://leetcode.com/problems/best-meeting-point/
// Author: A M A N
// Time : O(M*N)
// Space: O(M*N)
class Solution {
    vector<int> collectRows(vector<vector<int>>&grid){
        vector<int> rows;
        for(int i=0; i<grid.size(); i++)
            for(int j=0; j<grid[0].size(); j++)
                if(grid[i][j])
                    rows.push_back(i);
        return rows;
    }
    vector<int> collectCols(vector<vector<int>>&grid){
        vector<int> cols;
        for(int j=0; j<grid[0].size(); j++)
            for(int i=0; i<grid.size(); i++)
                if(grid[i][j])
                    cols.push_back(j);
        return cols;
    }
    int calculateDistance(vector<vector<int>>&grid, int x, int y){
        int distance = 0;
        for(int i=0; i<grid.size(); i++)
            for(int j=0; j<grid[0].size(); j++)
                if(grid[i][j])
                    distance+=abs(i-x) + abs(j-y);
        return distance;
    }
public:
    int minTotalDistance(vector<vector<int>>& grid) {
        vector<int> rows = collectRows(grid); // collects rows in non-decreasing order
        vector<int> cols = collectCols(grid); // collects cols in non-decreasing order
        int medX = rows[rows.size()/2];
        int medY = cols[cols.size()/2];
        return calculateDistance(grid, medX, medY);
    }
};
```