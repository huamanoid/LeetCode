# [587. Erect the Fence (Hard)](https://leetcode.com/problems/erect-the-fence/)

<p>You are given an array <code>trees</code> where <code>trees[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> represents the location of a tree in the garden.</p>

<p>You are asked to fence the entire garden using the minimum length of rope as it is expensive. The garden is well fenced only if <strong>all the trees are enclosed</strong>.</p>

<p>Return <em>the coordinates of trees that are exactly located on the fence perimeter</em>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/04/24/erect2-plane.jpg" style="width: 509px; height: 500px;">
<pre><strong>Input:</strong> points = [[1,1],[2,2],[2,0],[2,4],[3,3],[4,2]]
<strong>Output:</strong> [[1,1],[2,0],[3,3],[2,4],[4,2]]
</pre>

<p><strong>Example 2:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2021/04/24/erect1-plane.jpg" style="width: 509px; height: 500px;">
<pre><strong>Input:</strong> points = [[1,2],[2,2],[4,2]]
<strong>Output:</strong> [[4,2],[2,2],[1,2]]
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= points.length &lt;= 3000</code></li>
	<li><code>points[i].length == 2</code></li>
	<li><code>0 &lt;= x<sub>i</sub>, y<sub>i</sub> &lt;= 100</code></li>
	<li>All the given points are <strong>unique</strong>.</li>
</ul>


**Companies**:  
[Google](https://leetcode.com/company/google)

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Math](https://leetcode.com/tag/math/), [Geometry](https://leetcode.com/tag/geometry/)

**Similar Questions**:
* [Erect the Fence II (Hard)](https://leetcode.com/problems/erect-the-fence-ii/)

## Solution 1. Monotone chain Convex Hull Algorithm


1. Sort the points lexicographically
2. Wrap the bottom of the set of points by traversing left to right with convex like structure (remove points if it forms a concave structure) 
3. Similarly wrap the top by traversing from right to left on the sorted points

![](/assets/587.gif)

```cpp
// OJ: https://leetcode.com/problems/erect-the-fence/
// Author: A M A N
// Time : O(NlogN)
// Space: O(N)
class Solution {
    bool clockwise(vector<int>&a, vector<int>&b, vector<int>c){
        return (b[0]-a[0])*(c[1]-b[1]) - (b[1]-a[1])*(c[0]-b[0]) < 0;
    }
public:
    vector<vector<int>> outerTrees(vector<vector<int>>& points) {
        int n = points.size();
        sort(points.begin(), points.end(), [&](const vector<int>&a, const vector<int>&b){
            return a[0]==b[0]?a[1]<b[1]:a[0]<b[0];
        });
        vector<vector<int>> ans;
        // wrapping bottom (left to right)
        for(int i=0; i<points.size(); i++){ //clockwise orientation means a concave section (reluctant point included)
            while(ans.size()>=2 and clockwise(ans[ans.size()-2], ans[ans.size()-1], points[i]))
                ans.pop_back();
            ans.push_back(points[i]);
        }        
        if (ans.size() == n) return ans; // all points are rectilinear
        // wrapping the top (right to left)
        for(int i = points.size()-2; i>=0; i--){ // starting from second last point
            while(ans.size()>=2 and clockwise(ans[ans.size()-2], ans[ans.size()-1], points[i]))
                ans.pop_back();
            ans.push_back(points[i]);
        }
        ans.pop_back(); // the first point is included twice
        return ans;
    }
};
```