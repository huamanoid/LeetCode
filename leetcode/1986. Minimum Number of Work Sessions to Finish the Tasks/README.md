# [1986. Minimum Number of Work Sessions to Finish the Tasks (Medium)](https://leetcode.com/problems/minimum-number-of-work-sessions-to-finish-the-tasks/)

<p>There are <code>n</code> tasks assigned to you. The task times are represented as an integer array <code>tasks</code> of length <code>n</code>, where the <code>i<sup>th</sup></code> task takes <code>tasks[i]</code> hours to finish. A <strong>work session</strong> is when you work for <strong>at most</strong> <code>sessionTime</code> consecutive hours and then take a break.</p>

<p>You should finish the given tasks in a way that satisfies the following conditions:</p>

<ul>
	<li>If you start a task in a work session, you must complete it in the <strong>same</strong> work session.</li>
	<li>You can start a new task <strong>immediately</strong> after finishing the previous one.</li>
	<li>You may complete the tasks in <strong>any order</strong>.</li>
</ul>

<p>Given <code>tasks</code> and <code>sessionTime</code>, return <em>the <strong>minimum</strong> number of <strong>work sessions</strong> needed to finish all the tasks following the conditions above.</em></p>

<p>The tests are generated such that <code>sessionTime</code> is <strong>greater</strong> than or <strong>equal</strong> to the <strong>maximum</strong> element in <code>tasks[i]</code>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> tasks = [1,2,3], sessionTime = 3
<strong>Output:</strong> 2
<strong>Explanation:</strong> You can finish the tasks in two work sessions.
- First work session: finish the first and the second tasks in 1 + 2 = 3 hours.
- Second work session: finish the third task in 3 hours.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> tasks = [3,1,3,1,1], sessionTime = 8
<strong>Output:</strong> 2
<strong>Explanation:</strong> You can finish the tasks in two work sessions.
- First work session: finish all the tasks except the last one in 3 + 1 + 3 + 1 = 8 hours.
- Second work session: finish the last task in 1 hour.
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> tasks = [1,2,3,4,5], sessionTime = 15
<strong>Output:</strong> 1
<strong>Explanation:</strong> You can finish all the tasks in one work session.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>n == tasks.length</code></li>
	<li><code>1 &lt;= n &lt;= 14</code></li>
	<li><code>1 &lt;= tasks[i] &lt;= 10</code></li>
	<li><code>max(tasks[i]) &lt;= sessionTime &lt;= 15</code></li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/), [Backtracking](https://leetcode.com/tag/backtracking/), [Bit Manipulation](https://leetcode.com/tag/bit-manipulation/), [Bitmask](https://leetcode.com/tag/bitmask/)

**Similar Questions**:
* [Smallest Sufficient Team (Hard)](https://leetcode.com/problems/smallest-sufficient-team/)
* [Find Minimum Time to Finish All Jobs (Hard)](https://leetcode.com/problems/find-minimum-time-to-finish-all-jobs/)

## Solution 1. Backtracking + Memoization

```cpp
// OJ: https://leetcode.com/problems/minimum-number-of-work-sessions-to-finish-the-tasks/
// Author: A M A N
// Time : O()
// Space: O()
class Solution {
public:
    vector<int> buckets;
    map<int, map<vector<int>, int>> dp;

    int solve(vector<int>&tasks, int T, int idx){
        if(idx==tasks.size())
            return 0;
    
        if(dp.find(idx)!=dp.end() and dp[idx].find(buckets)!=dp[idx].end())
            return dp[idx][buckets];
    
        // creating a new time slot for the current task
        buckets.push_back(tasks[idx]);
        int ans = 1 + solve(tasks, T, idx+1);
        buckets.pop_back();
    
        // fitting the current task in each of the valid prev time slots
        for(int i=0; i<buckets.size(); i++){
            if(buckets[i]+tasks[idx]>T)continue;
            buckets[i]+=tasks[idx];
            ans = min(ans, solve(tasks, T, idx+1));             
            buckets[i]-=tasks[idx];
        }
    
        return dp[idx][buckets] = ans;
    }

    int minSessions(vector<int>& tasks, int T) {
        return solve(tasks, T, 0);
    }
};
```

saving some memory used by dp map by encoding the state to a string.

```cpp
// OJ: https://leetcode.com/problems/minimum-number-of-work-sessions-to-finish-the-tasks/
// Author: A M A N
// Time : O()
// Space: O()
class Solution {
public:
    vector<int> buckets;
    unordered_map<string,  int> dp;

    string encodeState(int idx){
        string s = to_string(idx)+"$";
        vector<int> copy = buckets;
        sort(copy.begin(), copy.end());
        for(auto i: copy)
            s+=to_string(i)+" ";
        return s;
    }
    int solve(vector<int>&tasks, int T, int idx){
        if(idx==tasks.size())
            return 0;
    
        string key = encodeState(idx);
        if(dp.find(key)!=dp.end())
            return dp[key];
    
        buckets.push_back(tasks[idx]);
        int ans = 1 + solve(tasks, T, idx+1);
        buckets.pop_back();
    
        for(int i=0; i<buckets.size(); i++){
            if(buckets[i]+tasks[idx]>T)continue;
            buckets[i]+=tasks[idx];
            ans = min(ans, solve(tasks, T, idx+1));             
            buckets[i]-=tasks[idx];
        }
        return dp[key] = ans;
    }

    int minSessions(vector<int>& tasks, int T) {
        return solve(tasks, T, 0);
    }
};
```
