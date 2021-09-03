# [774. Minimize Max Distance to Gas Station (Hard)](https://www.lintcode.com/problem/848/description)

<p>On a horizontal number line, we have gas stations at positions stations[0], stations[1], ..., stations[N-1], where N = stations.length.</p>

<p>Now, we add <code>K</code> more gas stations so that D, the maximum distance between adjacent gas stations, is minimized.</p>

<p>Return the <code>smallest</code> possible value of D.</p>

>Answers within 10^-6 of the true value will be accepted as correct.

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> stations = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]，K = 9
<strong>Output:</strong> 0.50
<strong>Explanation:</strong> The distance between adjacent gas stations is 0.50
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> stations = [3,6,12,19,33,44,67,72,89,95]，K = 2
<strong>Output:</strong> 14.00
<strong>Explanation:</strong> construction of gas stations at 86 locations
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>10 &lt;= n &lt;= 2000</code></li>
	<li><code>0 &lt;= stations[i] &lt;= 10<sup>8</sup></code></li>
	<li><code>1 &lt= K &lt;= 10<sup>6</sup></code></li>
    <li>Answers within 10^-6 of the true value will be accepted as correct.</li>
</ul>


## Solution 1. Binary Search on answers

```cpp
// OJ: https://leetcode.com/problems/minimize-max-distance-to-gas-station/
// Author: A M A N
// Time : O(NLogD) where D is diff b/w first and last station
// Space: O(1)
class Solution {
public:
    bool isValid(vector<int>&stations, int extra, double min){
        int cnt=0;
        for(int i=1; i<stations.size(); i++){
            double diff = stations[i]-stations[i-1];
            cnt += ceil(diff / min) - 1; 
        }
        return cnt<=extra;
    }
    double minmaxGasDist(vector<int> &stations, int k) {
        double ans = -1;
        double L = 0;
        double R = stations[stations.size()-1] - stations[0];
        double eps = 1e-6; // answers in this range are accepted
        while(L+eps<R){
            double M = L + (R-L)/2;
            if(isValid(stations, k, M)){
                ans = M;
                R = M;
            }else 
                L = M;
        }
        return ans;
    }
};
```