# [871. Minimum Number of Refueling Stops (Hard)](https://leetcode.com/problems/minimum-number-of-refueling-stops/)

<p>A car travels from a starting position to a destination which is <code>target</code> miles east of the starting position.</p>

<p>There are gas stations along the way. The gas stations are represented as an array <code>stations</code> where <code>stations[i] = [position<sub>i</sub>, fuel<sub>i</sub>]</code> indicates that the <code>i<sup>th</sup></code> gas station is <code>position<sub>i</sub></code> miles east of the starting position and has <code>fuel<sub>i</sub></code> liters of gas.</p>

<p>The car starts with an infinite tank of gas, which initially has <code>startFuel</code> liters of fuel in it. It uses one liter of gas per one mile that it drives. When the car reaches a gas station, it may stop and refuel, transferring all the gas from the station into the car.</p>

<p>Return <em>the minimum number of refueling stops the car must make in order to reach its destination</em>. If it cannot reach the destination, return <code>-1</code>.</p>

<p>Note that if the car reaches a gas station with <code>0</code> fuel left, the car can still refuel there. If the car reaches the destination with <code>0</code> fuel left, it is still considered to have arrived.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> target = 1, startFuel = 1, stations = []
<strong>Output:</strong> 0
<strong>Explanation:</strong> We can reach the target without refueling.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> target = 100, startFuel = 1, stations = [[10,100]]
<strong>Output:</strong> -1
<strong>Explanation:</strong> We can not reach the target (or even the first gas station).
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> target = 100, startFuel = 10, stations = [[10,60],[20,30],[30,30],[60,40]]
<strong>Output:</strong> 2
<strong>Explanation:</strong> We start with 10 liters of fuel.
We drive to position 10, expending 10 liters of fuel.  We refuel from 0 liters to 60 liters of gas.
Then, we drive from position 10 to position 60 (expending 50 liters of fuel),
and refuel from 10 liters to 50 liters of gas.  We then drive to and reach the target.
We made 2 refueling stops along the way, so we return 2.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= target, startFuel &lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt;= stations.length &lt;= 500</code></li>
	<li><code>0 &lt;= position<sub>i</sub> &lt;= position<sub>i+1</sub> &lt; target</code></li>
	<li><code>1 &lt;= fuel<sub>i</sub> &lt; 10<sup>9</sup></code></li>
</ul>


**Companies**:  
[Google](https://leetcode.com/company/google), [Flipkart](https://leetcode.com/company/flipkart), [Amazon](https://leetcode.com/company/amazon), [Uber](https://leetcode.com/company/uber), [Adobe](https://leetcode.com/company/adobe), [Morgan Stanley](https://leetcode.com/company/morgan-stanley), [Apple](https://leetcode.com/company/apple), [Fleetx](https://leetcode.com/company/fleetx), [Snapchat](https://leetcode.com/company/snapchat)

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/), [Greedy](https://leetcode.com/tag/greedy/), [Heap (Priority Queue)](https://leetcode.com/tag/heap-priority-queue/)

## Solution 1. Recursion + Memoization 
>(TLE)
```cpp
// OJ: https://leetcode.com/problems/minimum-number-of-refueling-stops/
// Author: A M A N
// Time : O(N^2 * F) where N is length of stations and F is max fuel
// Space: O(N*F)
class Solution {
public:
    unordered_map<int, unordered_map<int, int>> dp;
    int solve(vector<vector<int>>&stations, int cur, int end, int fuel){
        if(cur+fuel>=end) return 0;
        if(dp.find(cur)!=dp.end() and dp[cur].find(fuel)!=dp[cur].end())
            return dp[cur][fuel];
        int ans = 1e9;
        for(int i=0; i<stations.size(); i++){
            if(stations[i][0]>cur and stations[i][0]<=cur+fuel){
                int fuelWasted = stations[i][0]-cur;
                int fuelGained = stations[i][1];
                int pick = 1 + solve(stations, stations[i][0], end, fuel-fuelWasted+fuelGained);
                ans = min(ans, pick);
            }
        }
        return dp[cur][fuel] = ans;
    }
    int minRefuelStops(int target, int startFuel, vector<vector<int>>& stations) {
        int stops = solve(stations, 0, target, startFuel);
        return stops>=1e9?-1:stops;
    }
};
```