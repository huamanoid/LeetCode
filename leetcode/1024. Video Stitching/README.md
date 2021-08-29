# [1024. Video Stitching (Medium)](https://leetcode.com/problems/video-stitching/)

<p>You are given a series of video clips from a sporting event that lasted <code>time</code> seconds. These video clips can be overlapping with each other and have varying lengths.</p>

<p>Each video clip is described by an array <code>clips</code> where <code>clips[i] = [start<sub>i</sub>, end<sub>i</sub>]</code> indicates that the ith clip started at <code>start<sub>i</sub></code> and ended at <code>end<sub>i</sub></code>.</p>

<p>We can cut these clips into segments freely.</p>

<ul>
	<li>For example, a clip <code>[0, 7]</code> can be cut into segments <code>[0, 1] + [1, 3] + [3, 7]</code>.</li>
</ul>

<p>Return <em>the minimum number of clips needed so that we can cut the clips into segments that cover the entire sporting event</em> <code>[0, time]</code>. If the task is impossible, return <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> clips = [[0,2],[4,6],[8,10],[1,9],[1,5],[5,9]], time = 10
<strong>Output:</strong> 3
<strong>Explanation:</strong> 
We take the clips [0,2], [8,10], [1,9]; a total of 3 clips.
Then, we can reconstruct the sporting event as follows:
We cut [1,9] into segments [1,2] + [2,8] + [8,9].
Now we have segments [0,2] + [2,8] + [8,10] which cover the sporting event [0, 10].
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> clips = [[0,1],[1,2]], time = 5
<strong>Output:</strong> -1
<strong>Explanation:</strong> We can't cover [0,5] with only [0,1] and [1,2].
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> clips = [[0,1],[6,8],[0,2],[5,6],[0,4],[0,3],[6,7],[1,3],[4,7],[1,4],[2,5],[2,6],[3,4],[4,5],[5,7],[6,9]], time = 9
<strong>Output:</strong> 3
<strong>Explanation:</strong> We can take clips [0,4], [4,7], and [6,9].
</pre>

<p><strong>Example 4:</strong></p>

<pre><strong>Input:</strong> clips = [[0,4],[2,8]], time = 5
<strong>Output:</strong> 2
<strong>Explanation:</strong> Notice you can have extra video after the event ends.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= clips.length &lt;= 100</code></li>
	<li><code>0 &lt;= start<sub>i</sub> &lt;= end<sub>i</sub> &lt;= 100</code></li>
	<li><code>1 &lt;= time &lt;= 100</code></li>
</ul>


**Companies**:  
[Amazon](https://leetcode.com/company/amazon)

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/), [Greedy](https://leetcode.com/tag/greedy/)

## Solution 1. Dynamic Programming (Recursion + Memoization)

```cpp
// OJ: https://leetcode.com/problems/video-stitching/
// Author: A M A N
// Time : O(NLogN)?
// Space: O(N)
class Solution {
public:
    int dp[101];
    int solve(vector<vector<int>>&clips, int time, int idx, int prevEnd){
        if(prevEnd>=time)
            return 0;
        if(idx>=clips.size())
            return 1e9;
        if(dp[prevEnd]!=-1)
            return dp[prevEnd];
        //skip;
        int A = solve(clips, time, idx+1, prevEnd);
        //pick
        int B=1e9;
        int curStart = clips[idx][0], curEnd = clips[idx][1];
        if(curStart<=prevEnd){ //the new clip must start at the same or past minutes
            B = 1 + solve(clips, time, idx+1, max(prevEnd, curEnd));
        }
        return dp[prevEnd] = min(A, B);
    }
    int videoStitching(vector<vector<int>>& clips, int time) {
        memset(dp, -1, sizeof dp);
        sort(clips.begin(), clips.end()); // sorting helps us to track only the current ending time in recursion
        int cnt = solve(clips, time, 0, 0);
        return cnt>=1e9 ? -1: cnt;
    }
};
```