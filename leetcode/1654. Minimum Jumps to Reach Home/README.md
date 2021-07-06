# [1654. Minimum Jumps to Reach Home (Medium)](https://leetcode.com/problems/minimum-jumps-to-reach-home/)

<p>A certain bug's home is on the x-axis at position <code>x</code>. Help them get there from position <code>0</code>.</p>

<p>The bug jumps according to the following rules:</p>

<ul>
	<li>It can jump exactly <code>a</code> positions <strong>forward</strong> (to the right).</li>
	<li>It can jump exactly <code>b</code> positions <strong>backward</strong> (to the left).</li>
	<li>It cannot jump backward twice in a row.</li>
	<li>It cannot jump to any <code>forbidden</code> positions.</li>
</ul>

<p>The bug may jump forward <strong>beyond</strong> its home, but it <strong>cannot jump</strong> to positions numbered with <strong>negative</strong> integers.</p>

<p>Given an array of integers <code>forbidden</code>, where <code>forbidden[i]</code> means that the bug cannot jump to the position <code>forbidden[i]</code>, and integers <code>a</code>, <code>b</code>, and <code>x</code>, return <em>the minimum number of jumps needed for the bug to reach its home</em>. If there is no possible sequence of jumps that lands the bug on position <code>x</code>, return <code>-1.</code></p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> forbidden = [14,4,18,1,15], a = 3, b = 15, x = 9
<strong>Output:</strong> 3
<strong>Explanation:</strong> 3 jumps forward (0 -&gt; 3 -&gt; 6 -&gt; 9) will get the bug home.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> forbidden = [8,3,16,6,12,20], a = 15, b = 13, x = 11
<strong>Output:</strong> -1
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> forbidden = [1,6,2,14,5,17,4], a = 16, b = 9, x = 7
<strong>Output:</strong> 2
<strong>Explanation:</strong> One jump forward (0 -&gt; 16) then one jump backward (16 -&gt; 7) will get the bug home.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= forbidden.length &lt;= 1000</code></li>
	<li><code>1 &lt;= a, b, forbidden[i] &lt;= 2000</code></li>
	<li><code>0 &lt;= x &lt;= 2000</code></li>
	<li>All the elements in <code>forbidden</code> are distinct.</li>
	<li>Position <code>x</code> is not forbidden.</li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/), [Breadth-First Search](https://leetcode.com/tag/breadth-first-search/)

## Solution 1. BFS

```cpp
// OJ: https://leetcode.com/problems/minimum-jumps-to-reach-home/
// Author: A M A N
// Time : O()
// Space: O()
class Solution {
public:
    int minimumJumps(vector<int>& forbidden, int a, int b, int x) {
        unordered_set<int> forbid(forbidden.begin(), forbidden.end());
        vector<vector<bool>>vis(2, vector<bool>(6000, false));
        //vis[0][i] => visited from left side
        //vis[1][i] => visited from right side
        queue<pair<int, bool>> q;
        q.push({0, 0});
        vis[0][0] = vis[1][0] = 1;
        int ans = 0;
        while(!q.empty()){
            int n = q.size();
            for(int i=0; i<n; i++){
                auto [cur, fromRight] = q.front(); q.pop(); // fromRight to nake sure to not go backwards again
                if(cur==x)
                    return ans;
                int forward  = cur + a;
                int backward = cur - b;
                if(forward < 6000 and !vis[0][forward] and !forbid.count(forward)){
                    q.push({forward, false});
                    vis[0][forward] = 1;
                }
                if(backward>=0 and !fromRight and !vis[1][backward] and !forbid.count(backward)){
                    q.push({backward, true});
                    vis[1][backward] = 1;
                }
            }
            ans++;
        }
        return -1;
    }
};
```


## Solution 2. Recursion 

```cpp
// OJ: https://leetcode.com/problems/minimum-jumps-to-reach-home/
// Author: A M A N
// Time : O()
// Space: O()
class Solution {
public:
    unordered_set<int> forbid;
    int dp[6001][2];
    int solve(int pos, int target, int a, int b, bool fromRight){
        if(pos==target)
            return 0;
        if(forbid.find(pos)!=forbid.end() or pos<0 or pos>6000)
            return 1e9;
        if(dp[pos][fromRight]!=-1)
            return dp[pos][fromRight];
        dp[pos][fromRight] = 1 + solve(pos+a, target, a, b, false); // can always move forward irrespective of fromRight true or false
        if(!fromRight)
            dp[pos][fromRight] = min(dp[pos][fromRight], 1 + solve(pos-b, target, a, b, true));
        return dp[pos][fromRight];
    }
    int minimumJumps(vector<int>& forbidden, int a, int b, int x) {
        for(auto a: forbidden) forbid.insert(a);
        memset(dp, -1, sizeof dp);
        int ans = solve(0, x, a, b, 0);
        return ans>=1e9 ? -1 : ans;        
    }
};
```