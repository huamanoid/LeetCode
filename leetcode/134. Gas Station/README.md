# [134. Gas Station (Medium)](https://leetcode.com/problems/gas-station/)

<p>There are <code>n</code> gas stations along a circular route, where the amount of gas at the <code>i<sup>th</sup></code> station is <code>gas[i]</code>.</p>

<p>You have a car with an unlimited gas tank and it costs <code>cost[i]</code> of gas to travel from the <code>i<sup>th</sup></code> station to its next <code>(i + 1)<sup>th</sup></code> station. You begin the journey with an empty tank at one of the gas stations.</p>

<p>Given two integer arrays <code>gas</code> and <code>cost</code>, return <em>the starting gas station's index if you can travel around the circuit once in the clockwise direction, otherwise return</em> <code>-1</code>. If there exists a solution, it is <strong>guaranteed</strong> to be <strong>unique</strong></p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> gas = [1,2,3,4,5], cost = [3,4,5,1,2]
<strong>Output:</strong> 3
<strong>Explanation:</strong>
Start at station 3 (index 3) and fill up with 4 unit of gas. Your tank = 0 + 4 = 4
Travel to station 4. Your tank = 4 - 1 + 5 = 8
Travel to station 0. Your tank = 8 - 2 + 1 = 7
Travel to station 1. Your tank = 7 - 3 + 2 = 6
Travel to station 2. Your tank = 6 - 4 + 3 = 5
Travel to station 3. The cost is 5. Your gas is just enough to travel back to station 3.
Therefore, return 3 as the starting index.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> gas = [2,3,4], cost = [3,4,3]
<strong>Output:</strong> -1
<strong>Explanation:</strong>
You can't start at station 0 or 1, as there is not enough gas to travel to the next station.
Let's start at station 2 and fill up with 4 unit of gas. Your tank = 0 + 4 = 4
Travel to station 0. Your tank = 4 - 3 + 2 = 3
Travel to station 1. Your tank = 3 - 3 + 3 = 3
You cannot travel back to station 2, as it requires 4 unit of gas but you only have 3.
Therefore, you can't travel around the circuit once no matter where you start.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>gas.length == n</code></li>
	<li><code>cost.length == n</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= gas[i], cost[i] &lt;= 10<sup>4</sup></code></li>
</ul>


**Companies**:  
[Amazon](https://leetcode.com/company/amazon), [Hotstar](https://leetcode.com/company/hotstar), [Google](https://leetcode.com/company/google), [Goldman Sachs](https://leetcode.com/company/goldman-sachs)

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Greedy](https://leetcode.com/tag/greedy/)

## Solution 1. Simulation using recursion

```cpp
// OJ: https://leetcode.com/problems/gas-station/
// Author: A M A N
// Time : O(N^2)
// Space: O(N)
class Solution {
public:
    bool solve(vector<int>&gas, vector<int>&cost, int cur, int end, int fuel, bool begun){
        if(fuel<0) return false;
        if(begun and cur==end) return true;
        // fill gas and move to next, if not possible return false
        int next =(cur + 1)%(int)(gas.size());
        return solve(gas, cost, next, end, fuel+gas[cur]-cost[cur], true);
    }
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        for(int i=0; i<gas.size(); i++)
            if(solve(gas, cost, i, i, 0, false))
                return i;
        return -1;
    }
};
```

## Solution 2. Greedy

```cpp
// OJ: https://leetcode.com/problems/gas-station/
// Author: A M A N
// Time : O(N)
// Space: O(1)
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
       int starting_point=0, curr_tank=0, total=0;
        for(int i=0;i<cost.size();i++){
            total+=gas[i]-cost[i];
            curr_tank+=gas[i]-cost[i];
            if(curr_tank<0){ // can not reach this station, begin from here
                starting_point=i+1;
                curr_tank=0;
            }
        }
        //gas required to complete a cylce - sum of all available gas >=0
        return total >=0 ? starting_point:-1; 
    }
};
```