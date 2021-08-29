# [1326. Minimum Number of Taps to Open to Water a Garden (Hard)](https://leetcode.com/problems/minimum-number-of-taps-to-open-to-water-a-garden/)

<p>There is a one-dimensional garden on the x-axis. The garden starts at the point <code>0</code> and ends at the point <code>n</code>. (i.e The length of the garden is <code>n</code>).</p>

<p>There are&nbsp;<code>n + 1</code> taps located&nbsp;at points <code>[0, 1, ..., n]</code> in the garden.</p>

<p>Given an integer <code>n</code> and an integer array <code>ranges</code> of length <code>n + 1</code> where <code>ranges[i]</code> (0-indexed) means the <code>i-th</code> tap can water the area <code>[i - ranges[i], i + ranges[i]]</code> if it was open.</p>

<p>Return <em>the minimum number of taps</em> that should be open to water the whole garden, If the garden cannot be watered return <strong>-1</strong>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2020/01/16/1685_example_1.png" style="width: 525px; height: 255px;">
<pre><strong>Input:</strong> n = 5, ranges = [3,4,1,1,0,0]
<strong>Output:</strong> 1
<strong>Explanation:</strong> The tap at point 0 can cover the interval [-3,3]
The tap at point 1 can cover the interval [-3,5]
The tap at point 2 can cover the interval [1,3]
The tap at point 3 can cover the interval [2,4]
The tap at point 4 can cover the interval [4,4]
The tap at point 5 can cover the interval [5,5]
Opening Only the second tap will water the whole garden [0,5]
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> n = 3, ranges = [0,0,0,0]
<strong>Output:</strong> -1
<strong>Explanation:</strong> Even if you activate all the four taps you cannot water the whole garden.
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> n = 7, ranges = [1,2,1,0,2,1,0,1]
<strong>Output:</strong> 3
</pre>

<p><strong>Example 4:</strong></p>

<pre><strong>Input:</strong> n = 8, ranges = [4,0,0,0,0,0,0,0,4]
<strong>Output:</strong> 2
</pre>

<p><strong>Example 5:</strong></p>

<pre><strong>Input:</strong> n = 8, ranges = [4,0,0,0,4,0,0,0,4]
<strong>Output:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10^4</code></li>
	<li><code>ranges.length == n + 1</code></li>
	<li><code>0 &lt;= ranges[i] &lt;= 100</code></li>
</ul>


**Companies**:  
[Facebook](https://leetcode.com/company/facebook), [Morgan Stanley](https://leetcode.com/company/morgan-stanley), [Google](https://leetcode.com/company/google), [Docusign](https://leetcode.com/company/docusign), [Apple](https://leetcode.com/company/apple), [DE Shaw](https://leetcode.com/company/de-shaw), [Mathworks](https://leetcode.com/company/mathworks), [Flipkart](https://leetcode.com/company/flipkart), [Lyft](https://leetcode.com/company/lyft)

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/), [Greedy](https://leetcode.com/tag/greedy/)

## Solution 1. Dynamic Programming

```cpp
// OJ: https://leetcode.com/problems/minimum-number-of-taps-to-open-to-water-a-garden/
// Author: A M A N
// Time : O(NlogN)?
// Space: O(N)
class Solution {
    vector<pair<int, int>>interval;
public:
    int dp[10001];
    int solve(int n, int idx, int prevEnd){
        if(prevEnd>=n)return 0;
        if(idx>=interval.size()) return 1e9;
        if(dp[prevEnd]!=-1)
            return dp[prevEnd];
        //skip
        int A = solve(n, idx+1, prevEnd);
        //pick
        int B = 1e9;
        int curStart = interval[idx].first, curEnd = interval[idx].second;
        if(curStart<=prevEnd){
            B = 1 + solve(n, idx+1, max(curEnd, prevEnd));
        }
        return dp[prevEnd] = min(A, B);
    }
    int minTaps(int n, vector<int>& ranges) {
        memset(dp, -1, sizeof dp);
        for(int i=0; i<ranges.size(); i++)
            interval.push_back({max(0, i-ranges[i]), min(n, i+ranges[i])});
        sort(interval.begin(), interval.end());
        int cnt = solve(n, 0, 0);
        return cnt>=1e9?-1:cnt;
    }
};
```