# [1335. Minimum Difficulty of a Job Schedule (Hard)](https://leetcode.com/problems/minimum-difficulty-of-a-job-schedule/)

<p>You want to schedule a list of jobs in <code>d</code> days. Jobs are dependent (i.e To work on the <code>i-th</code> job, you have to finish all the jobs <code>j</code> where <code>0 &lt;= j &lt; i</code>).</p>

<p>You have to finish <strong>at least</strong> one task every day. The difficulty of a job schedule is the sum of difficulties of each day of the <code>d</code> days. The difficulty of a day is the maximum difficulty of a job done in that day.</p>

<p>Given an array of integers <code>jobDifficulty</code> and an integer <code>d</code>. The difficulty of the <code>i-th</code>&nbsp;job is&nbsp;<code>jobDifficulty[i]</code>.</p>

<p>Return <em>the minimum difficulty</em> of a job schedule. If you cannot find a schedule for the jobs return <strong>-1</strong>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2020/01/16/untitled.png" style="width: 365px; height: 370px;">
<pre><strong>Input:</strong> jobDifficulty = [6,5,4,3,2,1], d = 2
<strong>Output:</strong> 7
<strong>Explanation:</strong> First day you can finish the first 5 jobs, total difficulty = 6.
Second day you can finish the last job, total difficulty = 1.
The difficulty of the schedule = 6 + 1 = 7 
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> jobDifficulty = [9,9,9], d = 4
<strong>Output:</strong> -1
<strong>Explanation:</strong> If you finish a job per day you will still have a free day. you cannot find a schedule for the given jobs.
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> jobDifficulty = [1,1,1], d = 3
<strong>Output:</strong> 3
<strong>Explanation:</strong> The schedule is one job per day. total difficulty will be 3.
</pre>

<p><strong>Example 4:</strong></p>

<pre><strong>Input:</strong> jobDifficulty = [7,1,7,1,7,1], d = 3
<strong>Output:</strong> 15
</pre>

<p><strong>Example 5:</strong></p>

<pre><strong>Input:</strong> jobDifficulty = [11,111,22,222,33,333,44,444], d = 6
<strong>Output:</strong> 843
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= jobDifficulty.length &lt;= 300</code></li>
	<li><code>0 &lt;= jobDifficulty[i] &lt;= 1000</code></li>
	<li><code>1 &lt;= d &lt;= 10</code></li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/)

## Solution 1. DP

```cpp
// OJ: https://leetcode.com/problems/minimum-difficulty-of-a-job-schedule/
// Author: A M A N
// Time : O(D * N * N * N)
// Space: O(D * N)
class Solution {
public:
    int maxInRange(vector<int>&jd, int start, int end){
        int res = INT_MIN;
        for(int i=start; i<=end; i++)
            res = max(res, jd[i]);
        return res;
    }
    int minDifficulty(vector<int>& jd, int d) {
        int n = jd.size();
        int dp[d][n];
        memset(dp, -1, sizeof dp);
        dp[0][0] = jd[0];
        for(int j=1; j<n; j++)  dp[0][j] = max(dp[0][j-1], jd[j]);

        for(int i=1; i<d; i++){
            for(int j=i; j<n; j++){
                if(j==i)
                    dp[i][j] = jd[j] + dp[i-1][j-1]; // redundant but intuitive
                else {
                    int res = INT_MAX;
                    for(int k=j; k>=i; k--)
                        res = min(res, dp[i-1][k-1] + maxInRange(jd, k, j));
                    dp[i][j] = res;
                }    
            }
        }
        return dp[d-1][n-1];
    }
};
```

Optimizing it using `pre-computed` maximum in range values.

```cpp
// OJ: https://leetcode.com/problems/minimum-difficulty-of-a-job-schedule/
// Author: A M A N
// Time : O(D * N * N)
// Space: O(N * N)
class Solution {
public:
    int minDifficulty(vector<int>& jd, int d) {
        int n = jd.size();
        int dp[d][n];
        memset(dp, -1, sizeof dp);
        dp[0][0] = jd[0];
        for(int j=1; j<n; j++)  dp[0][j] = max(dp[0][j-1], jd[j]);
        int maximumInRange[n][n];
        for(int i=0; i<n; i++){
            int maxSoFar = INT_MIN;
            for(int j=i; j<n; j++)
                maximumInRange[i][j] = maxSoFar = max(maxSoFar, jd[j]);
        }
        for(int i=1; i<d; i++){
            for(int j=i; j<n; j++){
                int res = INT_MAX;
                for(int k=j; k>=i; k--) 
                    res = min(res, dp[i-1][k-1] + maximumInRange[k][j]);
                dp[i][j] = res;
            }
        }
        return dp[d-1][n-1];
    }
};
```