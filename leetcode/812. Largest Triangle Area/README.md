# [812. Largest Triangle Area (Easy)](https://leetcode.com/problems/largest-triangle-area/)

<p>Given an array of points on the <strong>X-Y</strong> plane <code>points</code> where <code>points[i] = [x<sub>i</sub>, y<sub>i</sub>]</code>, return <em>the area of the largest triangle that can be formed by any three different points</em>. Answers within <code>10<sup>-5</sup></code> of the actual answer will be accepted.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://s3-lc-upload.s3.amazonaws.com/uploads/2018/04/04/1027.png" style="height: 369px; width: 450px;">
<pre><strong>Input:</strong> points = [[0,0],[0,1],[1,0],[0,2],[2,0]]
<strong>Output:</strong> 2.00000
<strong>Explanation:</strong> The five points are shown in the above figure. The red triangle is the largest.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> points = [[1,0],[0,0],[0,1]]
<strong>Output:</strong> 0.50000
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>3 &lt;= points.length &lt;= 50</code></li>
	<li><code>-50 &lt;= x<sub>i</sub>, y<sub>i</sub> &lt;= 50</code></li>
	<li>All the given points are <strong>unique</strong>.</li>
</ul>


**Companies**:  
[Google](https://leetcode.com/company/google)

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Math](https://leetcode.com/tag/math/), [Geometry](https://leetcode.com/tag/geometry/)

**Similar Questions**:
* [Largest Perimeter Triangle (Easy)](https://leetcode.com/problems/largest-perimeter-triangle/)

## Solution 1. Brute-force

```cpp
// OJ: https://leetcode.com/problems/largest-triangle-area/
// Author: A M A N
// Time : O(N^3)
// Space: O(1)
class Solution {
public:
    //area = abs(x1*(y2-y3) + x2*(y3-y1) + x3*(y1-y2))/2;
    double area(vector<int> a, vector<int> b, vector<int> c){
        return abs(a[0]*(b[1]-c[1]) + b[0]*(c[1]-a[1]) + c[0]*(a[1]-b[1]))/2.00;
    }
    double largestTriangleArea(vector<vector<int>>& points) {
        double maxArea = 0;
        int n = points.size();
        for(int i=0; i<n; i++){
            for(int j=i+1; j<n; j++){
                for(int k=j+1; k<n; k++){
                    maxArea = max(maxArea, area(points[i], points[j], points[k]));
                }
            }
        }
        return maxArea;
    }
};
```