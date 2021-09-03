# [935. Knight Dialer (Medium)](https://leetcode.com/problems/knight-dialer/)

<p>The chess knight has a <strong>unique movement</strong>,&nbsp;it may move two squares vertically and one square horizontally, or two squares horizontally and one square vertically (with both forming the shape of an <strong>L</strong>). The possible movements of chess knight are shown in this diagaram:</p>

<p>A chess knight can move as indicated in the chess diagram below:</p>
<img alt="" src="https://assets.leetcode.com/uploads/2020/08/18/chess.jpg" style="width: 402px; height: 402px;">
<p>We have a chess knight and a phone pad as shown below, the knight <strong>can only stand on a numeric cell</strong>&nbsp;(i.e. blue cell).</p>
<img alt="" src="https://assets.leetcode.com/uploads/2020/08/18/phone.jpg" style="width: 242px; height: 322px;">
<p>Given an integer <code>n</code>, return how many distinct phone numbers of length <code>n</code> we can dial.</p>

<p>You are allowed to place the knight <strong>on any numeric cell</strong> initially and then you should perform <code>n - 1</code> jumps to dial a number of length <code>n</code>. All jumps should be <strong>valid</strong> knight jumps.</p>

<p>As the answer may be very large, <strong>return the answer modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> n = 1
<strong>Output:</strong> 10
<strong>Explanation:</strong> We need to dial a number of length 1, so placing the knight over any numeric cell of the 10 cells is sufficient.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> n = 2
<strong>Output:</strong> 20
<strong>Explanation:</strong> All the valid number we can dial are [04, 06, 16, 18, 27, 29, 34, 38, 40, 43, 49, 60, 61, 67, 72, 76, 81, 83, 92, 94]
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> n = 3
<strong>Output:</strong> 46
</pre>

<p><strong>Example 4:</strong></p>

<pre><strong>Input:</strong> n = 4
<strong>Output:</strong> 104
</pre>

<p><strong>Example 5:</strong></p>

<pre><strong>Input:</strong> n = 3131
<strong>Output:</strong> 136006598
<strong>Explanation:</strong> Please take care of the mod.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 5000</code></li>
</ul>


**Related Topics**:  
[Dynamic Programming](https://leetcode.com/tag/dynamic-programming/)

## Solution 1. Recursion + Memoization

```cpp
// OJ: https://leetcode.com/problems/knight-dialer/
// Author: A M A N
// Time : O(N*logN)
// Space: O(N)
class Solution {
public:
    const int M = 1e9+7;
    unordered_map<int, vector<int>> mp;
    map<pair<int, int>, int> dp;    
    int solve(int n, int pos){
        if(n==1)
            return 1;
        if(dp.find({n, pos})!=dp.end())
            return dp[{n, pos}];
        int res = 0;
        for(auto next: mp[pos])
            (res+= solve(n-1, next))%=M;
        return dp[{n, pos}] = res;
    }
    int knightDialer(int n) {
        mp[1] = {6, 8};
        mp[2] = {7, 9};
        mp[3] = {4, 8};
        mp[4] = {0, 3, 9};
        mp[5] = {};
        mp[6] = {0, 1, 7};
        mp[7] = {2, 6};
        mp[8] = {1, 3};
        mp[9] = {2, 4};
        mp[0] = {4, 6};
        int ans=0;
        for(int i=0; i<=9; i++){
            (ans += solve(n, i))%=M;
        }
        return ans;
    }
};
```

## Solution 2. DP

```cpp
// OJ: https://leetcode.com/problems/knight-dialer/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    const int M = 1e9+7;
    unordered_map<int, vector<int>> mp;
    int knightDialer(int n) {
        mp[1] = {6, 8};
        mp[2] = {7, 9};
        mp[3] = {4, 8};
        mp[4] = {0, 3, 9};
        mp[5] = {};
        mp[6] = {0, 1, 7};
        mp[7] = {2, 6};
        mp[8] = {1, 3};
        mp[9] = {2, 4};
        mp[0] = {4, 6};
        int dp[n][10];
        memset(dp, 0, sizeof dp);
        for(int j=0; j<10; j++)dp[0][j] = 1;
        for(int i=1; i<n; i++){
            for(int j=0; j<10; j++){
                for(auto next: mp[j])
                    (dp[i][j]+= dp[i-1][next])%=M;
            }
        }
        return accumulate(dp[n-1], dp[n-1] + 10, 0LL)%M;
    }
};
```