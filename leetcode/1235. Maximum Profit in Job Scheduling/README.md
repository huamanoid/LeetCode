# [1235. Maximum Profit in Job Scheduling (Hard)](https://leetcode.com/problems/maximum-profit-in-job-scheduling/)

<p>We have <code>n</code> jobs, where every job is scheduled to be done from <code>startTime[i]</code> to <code>endTime[i]</code>, obtaining a profit of <code>profit[i]</code>.</p>

<p>You're given the <code>startTime</code>, <code>endTime</code> and <code>profit</code> arrays, return the maximum profit you can take such that there are no two jobs in the subset with overlapping time range.</p>

<p>If you choose a job that ends at time <code>X</code> you will be able to start another job that starts at time <code>X</code>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<p><strong><img alt="" src="https://assets.leetcode.com/uploads/2019/10/10/sample1_1584.png" style="width: 380px; height: 154px;"></strong></p>

<pre><strong>Input:</strong> startTime = [1,2,3,3], endTime = [3,4,5,6], profit = [50,10,40,70]
<strong>Output:</strong> 120
<strong>Explanation:</strong> The subset chosen is the first and fourth job. 
Time range [1-3]+[3-6] , we get profit of 120 = 50 + 70.
</pre>

<p><strong>Example 2:</strong></p>

<p><strong><img alt="" src="https://assets.leetcode.com/uploads/2019/10/10/sample22_1584.png" style="width: 600px; height: 112px;"> </strong></p>

<pre><strong>Input:</strong> startTime = [1,2,3,4,6], endTime = [3,5,10,6,9], profit = [20,20,100,70,60]
<strong>Output:</strong> 150
<strong>Explanation:</strong> The subset chosen is the first, fourth and fifth job. 
Profit obtained 150 = 20 + 70 + 60.
</pre>

<p><strong>Example 3:</strong></p>

<p><strong><img alt="" src="https://assets.leetcode.com/uploads/2019/10/10/sample3_1584.png" style="width: 400px; height: 112px;"></strong></p>

<pre><strong>Input:</strong> startTime = [1,1,1], endTime = [2,3,4], profit = [5,6,4]
<strong>Output:</strong> 6
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= startTime.length == endTime.length == profit.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= startTime[i] &lt; endTime[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= profit[i] &lt;= 10<sup>4</sup></code></li>
</ul>


**Companies**:  
[DoorDash](https://leetcode.com/company/doordash), [Amazon](https://leetcode.com/company/amazon), [Swiggy](https://leetcode.com/company/swiggy), [Databricks](https://leetcode.com/company/databricks), [Airbnb](https://leetcode.com/company/airbnb), [Microsoft](https://leetcode.com/company/microsoft), [LinkedIn](https://leetcode.com/company/linkedin), [Adobe](https://leetcode.com/company/adobe)

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Binary Search](https://leetcode.com/tag/binary-search/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/), [Sorting](https://leetcode.com/tag/sorting/)

## Solution 1. (Recursion + Memoizaiton) and BinarySearch

```cpp
// OJ: https://leetcode.com/problems/maximum-profit-in-job-scheduling/
// Author: A M A N
// Time : O(NlogN)
// Space: O(N)
class Solution {
public:
    vector<pair<int, int>> start;
    vector<int>end, P;
    int dp[50001];
    int solve(int idx){
        if(idx>=start.size())
            return 0;
        if(dp[idx]!=-1)
            return dp[idx];
        //skip
        int a = solve(idx+1); // not choose the current job schedule
        //pick
        int originalIdx = start[idx].second; // original index (before sorting) to get the corresponding ending time and profit
        //  if we choose the current schedule then we can only be free after this ends
        int nextIdx  = lower_bound(start.begin(), start.end(), make_pair(end[originalIdx],-1)) - start.begin();
        return dp[idx] = max(a, P[originalIdx] + solve(nextIdx));
    }
    int jobScheduling(vector<int>& startTime, vector<int>& endTime, vector<int>& profit) {
        end = endTime, P = profit;
        for(int i=0; i<startTime.size(); i++)
            start.push_back({startTime[i], i}); // to preserve the orginal indices after sorting
        sort(start.begin(), start.end()); // sort wrt to the starting time 
        memset(dp, -1, sizeof dp);
        return solve(0);
    }
};
```