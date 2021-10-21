# [1723. Find Minimum Time to Finish All Jobs (Hard)](https://leetcode.com/problems/find-minimum-time-to-finish-all-jobs/)

<p>You are given an integer array <code>jobs</code>, where <code>jobs[i]</code> is the amount of time it takes to complete the <code>i<sup>th</sup></code> job.</p>

<p>There are <code>k</code> workers that you can assign jobs to. Each job should be assigned to <strong>exactly</strong> one worker. The <strong>working time</strong> of a worker is the sum of the time it takes to complete all jobs assigned to them. Your goal is to devise an optimal assignment such that the <strong>maximum working time</strong> of any worker is <strong>minimized</strong>.</p>

<p><em>Return the <strong>minimum</strong> possible <strong>maximum working time</strong> of any assignment. </em></p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> jobs = [3,2,3], k = 3
<strong>Output:</strong> 3
<strong>Explanation:</strong> By assigning each person one job, the maximum time is 3.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> jobs = [1,2,4,7,8], k = 2
<strong>Output:</strong> 11
<strong>Explanation:</strong> Assign the jobs the following way:
Worker 1: 1, 2, 8 (working time = 1 + 2 + 8 = 11)
Worker 2: 4, 7 (working time = 4 + 7 = 11)
The maximum working time is 11.</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= jobs.length &lt;= 12</code></li>
	<li><code>1 &lt;= jobs[i] &lt;= 10<sup>7</sup></code></li>
</ul>


**Companies**:  
[Amazon](https://leetcode.com/company/amazon)

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/), [Backtracking](https://leetcode.com/tag/backtracking/), [Bit Manipulation](https://leetcode.com/tag/bit-manipulation/), [Bitmask](https://leetcode.com/tag/bitmask/)

**Similar Questions**:
* [Minimum Number of Work Sessions to Finish the Tasks (Medium)](https://leetcode.com/problems/minimum-number-of-work-sessions-to-finish-the-tasks/)

## Solution 1. Backtracking

```cpp
// OJ: https://leetcode.com/problems/find-minimum-time-to-finish-all-jobs/
// Author: A M A N
// Time : O(K^N)
// Space: O(NK)
class Solution {
public:
    int ans = 1e9;
    vector<int>buckets;
    void solve(vector<int>&jobs, int idx){
        if(idx>=jobs.size()) {
            int mx = *max_element(buckets.begin(), buckets.end());
            ans = min(ans, mx);
            return;
        }
        unordered_set<int>seen;
        for(int i=0; i<buckets.size(); i++){
            if(seen.count(buckets[i]))continue;
            if(buckets[i]+jobs[idx]>ans) continue;
            seen.insert(buckets[i]);
            buckets[i]+=jobs[idx];
            solve(jobs, idx+1);
            buckets[i]-=jobs[idx];
        }
    }
    int minimumTimeRequired(vector<int>& jobs, int k) {
        buckets.resize(k, 0);
        sort(jobs.begin(), jobs.end());
        solve(jobs, 0);
        return ans;
    }
};
```
Another type of optimization is preventing assign a work A[i] to totally free workers twice because assigning to either totally free worker will get the same result.

The time complexity of brute force DFS is O(K^N). If a DFS invocation sees x zeros, this optimization will skip x - 1 zeros and thus reduce the time complexity to the 1/x of the brute force one. In the first layer of DFS, there are K zeros. In the second layer, there are at least K - 1 zeros. In the third layer, there are at least K - 2 zeros, and so on. So the time complexity will be 1/K * 1/(K - 1) * 1/(K - 2) * ... * 1/1 = 1/K! of the brute force one, i.e. O(K^N / K!).

```cpp
// OJ: https://leetcode.com/problems/find-minimum-time-to-finish-all-jobs/
// Author: A M A N
// Time : O(K^N/K!)
// Space: O(N)
class Solution {
public:
    int ans = 1e9;
    vector<int>buckets;
    void solve(vector<int>&jobs, int idx){
        if(idx>=jobs.size()) {
            int mx = *max_element(buckets.begin(), buckets.end());
            ans = min(ans, mx);
            return;
        }
        for(int i=0; i<buckets.size(); i++){
            if(buckets[i]+jobs[idx]>ans) continue;
            buckets[i]+=jobs[idx];
            solve(jobs, idx+1);
            buckets[i]-=jobs[idx];
            if(buckets[i]==0)break; // prevent assigning `jobs[idx]` to other totally free workers `buckets[i + 1]`, `buckets[i + 2]`, ...
        }
    }
    int minimumTimeRequired(vector<int>& jobs, int k) {
        buckets.resize(k, 0);
        sort(jobs.begin(), jobs.end());
        solve(jobs, 0);
        return ans;
    }
};
```

## Solution 2. Binary Search + DFS

The range of the answer is [max(A), sum(A)]. We can binary search in this range.

Let dfs(limit) be whether we can fit the jobs with less than or equal to k workers given the maximum working time limit.

```cpp
// OJ: https://leetcode.com/problems/find-minimum-time-to-finish-all-jobs/
// Author: A M A N
// Time : O()
// Space: O()
class Solution {
public:
    int minimumTimeRequired(vector<int>& jobs, int k) {
        sort(jobs.begin(), jobs.end(), greater<int>());
        vector<int> buckets(k, 0);
        
        function<bool(int, int)> dfs = [&](int idx, int mx){
            if(idx==jobs.size()) return true;
            for(int i=0; i<k; i++){
                if(buckets[i] + jobs[idx] > mx) continue;
                buckets[i]+=jobs[idx];
                if(dfs(idx+1, mx))
                    return true;
                buckets[i]-=jobs[idx];
                if(buckets[i]==0)break;
            }
            return false;
        };

        int L = *max_element(jobs.begin(), jobs.end());
        int R = accumulate(jobs.begin(), jobs.end(), 0);
        while(L<=R){
            int M = L + (R-L)/2;
            buckets.assign(k, 0);
            if(dfs(0, M))
                R = M-1;
            else
                L = M+1;
        }
        return L;
    }
};
```
