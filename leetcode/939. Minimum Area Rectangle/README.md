# [939. Minimum Area Rectangle (Medium)](https://leetcode.com/problems/minimum-area-rectangle/)

<p>You are given an array of points in the <strong>X-Y</strong> plane <code>points</code> where <code>points[i] = [x<sub>i</sub>, y<sub>i</sub>]</code>.</p>

<p>Return <em>the minimum area of a rectangle formed from these points, with sides parallel to the X and Y axes</em>. If there is not any such rectangle, return <code>0</code>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/08/03/rec1.JPG" style="width: 500px; height: 447px;">
<pre><strong>Input:</strong> points = [[1,1],[1,3],[3,1],[3,3],[2,2]]
<strong>Output:</strong> 4
</pre>

<p><strong>Example 2:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/08/03/rec2.JPG" style="width: 500px; height: 477px;">
<pre><strong>Input:</strong> points = [[1,1],[1,3],[3,1],[3,3],[4,1],[4,3]]
<strong>Output:</strong> 2
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= points.length &lt;= 500</code></li>
	<li><code>points[i].length == 2</code></li>
	<li><code>0 &lt;= x<sub>i</sub>, y<sub>i</sub> &lt;= 4 * 10<sup>4</sup></code></li>
	<li>All the given points are <strong>unique</strong>.</li>
</ul>


**Companies**:  
[Google](https://leetcode.com/company/google), [Facebook](https://leetcode.com/company/facebook), [Snapchat](https://leetcode.com/company/snapchat), [Microsoft](https://leetcode.com/company/microsoft)

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Hash Table](https://leetcode.com/tag/hash-table/), [Math](https://leetcode.com/tag/math/), [Geometry](https://leetcode.com/tag/geometry/), [Sorting](https://leetcode.com/tag/sorting/)

## Solution 1.

```cpp
// OJ: https://leetcode.com/problems/minimum-area-rectangle/
// Author: A M A N
// Time : O(N*N*logN)
// Space: O(N)
class Solution {
public:
    int minAreaRect(vector<vector<int>>& points) {
        set<pair<int, int> > pts;
        for(vector<int> &p : points){
            pts.insert({p[0], p[1]});
        }
        int area = INT_MAX;
        for(auto &p : pts){
            for(auto &q : pts){
                // p and q are not diagonal points
                if(p.first == q.first or p.second == q.second) continue; 
                // p and q are diagonal points
                if(pts.count({p.first, q.second}) and pts.count({q.first, p.second})){
                    area = min(area, abs((p.first - q.first) * (p.second - q.second)));
                }
            }
        }
        return area==INT_MAX?0:area;
    }
};
```