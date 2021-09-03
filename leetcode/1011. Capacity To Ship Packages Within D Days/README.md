# [1011. Capacity To Ship Packages Within D Days (Medium)](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/)

<p>A conveyor belt has packages that must be shipped from one port to another within <code>days</code> days.</p>

<p>The <code>i<sup>th</sup></code> package on the conveyor belt has a weight of <code>weights[i]</code>. Each day, we load the ship with packages on the conveyor belt (in the order given by <code>weights</code>). We may not load more weight than the maximum weight capacity of the ship.</p>

<p>Return the least weight capacity of the ship that will result in all the packages on the conveyor belt being shipped within <code>days</code> days.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> weights = [1,2,3,4,5,6,7,8,9,10], days = 5
<strong>Output:</strong> 15
<strong>Explanation:</strong> A ship capacity of 15 is the minimum to ship all the packages in 5 days like this:
1st day: 1, 2, 3, 4, 5
2nd day: 6, 7
3rd day: 8
4th day: 9
5th day: 10

Note that the cargo must be shipped in the order given, so using a ship of capacity 14 and splitting the packages into parts like (2, 3, 4, 5), (1, 6, 7), (8), (9), (10) is not allowed.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> weights = [3,2,2,4,1,4], days = 3
<strong>Output:</strong> 6
<strong>Explanation:</strong> A ship capacity of 6 is the minimum to ship all the packages in 3 days like this:
1st day: 3, 2
2nd day: 2, 4
3rd day: 1, 4
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> weights = [1,2,3,1,1], days = 4
<strong>Output:</strong> 3
<strong>Explanation:</strong>
1st day: 1
2nd day: 2
3rd day: 3
4th day: 1, 1
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= days &lt;= weights.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= weights[i] &lt;= 500</code></li>
</ul>

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Binary Search](https://leetcode.com/tag/binary-search/), [Greedy](https://leetcode.com/tag/greedy/)

**Similar Questions**:
* [Cutting Ribbons (Medium)](https://leetcode.com/problems/cutting-ribbons/)

## Solution 1. Binary Search on answers

```cpp
// OJ: https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/
// Author: A M A N
// Time : O(log(S)) where S is sum of all the weights
// Space: O(1)
class Solution {
public:
    bool isValid(vector<int>& weights, int days, int mx){
        int sum =0;
        int cnt =1;
        for(int i=0;i<weights.size();i++){
            if(weights[i]>mx)
                    return false;
            sum+=weights[i];
            if(sum>mx){
                sum= weights[i];
                cnt++;
            }
        }
        return days>=cnt;
    }
    int shipWithinDays(vector<int>& weights, int days) {
        int low = 0;
        int high = accumulate(weights.begin(), weights.end(), 0);
        int ans = INT_MAX;
        while(low<=high){
            int mid = low + (high-low)/2;
            if(isValid(weights, days, mid)){
                ans = mid; // ans will be minimized as the search progresses
                high = mid-1;
            }else
                low = mid+1;
        }
        return ans;
    }
};
```