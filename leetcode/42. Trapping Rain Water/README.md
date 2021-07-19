# [42. Trapping Rain Water (Hard)](https://leetcode.com/problems/trapping-rain-water/)

<p>Given <code>n</code> non-negative integers representing an elevation map where the width of each bar is <code>1</code>, compute how much water it can trap after raining.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img src="https://assets.leetcode.com/uploads/2018/10/22/rainwatertrap.png" style="width: 412px; height: 161px;">
<pre><strong>Input:</strong> height = [0,1,0,2,1,0,1,3,2,1,2,1]
<strong>Output:</strong> 6
<strong>Explanation:</strong> The above elevation map (black section) is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> height = [4,2,0,3,2,5]
<strong>Output:</strong> 9
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>n == height.length</code></li>
	<li><code>0 &lt;= n &lt;= 3 * 10<sup>4</sup></code></li>
	<li><code>0 &lt;= height[i] &lt;= 10<sup>5</sup></code></li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Two Pointers](https://leetcode.com/tag/two-pointers/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/), [Stack](https://leetcode.com/tag/stack/), [Monotonic Stack](https://leetcode.com/tag/monotonic-stack/)

**Similar Questions**:
* [Container With Most Water (Medium)](https://leetcode.com/problems/container-with-most-water/)
* [Product of Array Except Self (Medium)](https://leetcode.com/problems/product-of-array-except-self/)
* [Trapping Rain Water II (Hard)](https://leetcode.com/problems/trapping-rain-water-ii/)
* [Pour Water (Medium)](https://leetcode.com/problems/pour-water/)

## Solution 1. Monotonic Stack

```cpp
// OJ: https://leetcode.com/problems/trapping-rain-water/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    int trap(vector<int>& h) {
        stack<pair<int, int>> st; // storing only decreasing values
        int water=0;
        for(int i=0; i<h.size(); i++){
            while(st.size() and h[i] > st.top().first){
                int prev = st.top().first;
                st.pop();
                if(st.size()) // height above previously added water level * width
                    water+= (min(st.top().first, h[i]) - prev) * (i - st.top().second - 1);
            }
            st.push({h[i], i});
        }
        return water;
    }
};
```