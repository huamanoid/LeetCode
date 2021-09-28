# [1434. Number of Ways to Wear Different Hats to Each Other (Hard)](https://leetcode.com/problems/number-of-ways-to-wear-different-hats-to-each-other/)

<p>There are&nbsp;<code>n</code> people&nbsp;and 40 types of hats labeled from 1 to 40.</p>

<p>Given a list of list of integers <code>hats</code>, where <code>hats[i]</code>&nbsp;is a list of all hats preferred&nbsp;by the <code data-stringify-type="code">i-th</code> person.</p>

<p>Return the number of ways that the n people wear different hats to each other.</p>

<p>Since the answer&nbsp;may be too large,&nbsp;return it modulo&nbsp;<code>10^9 + 7</code>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> hats = [[3,4],[4,5],[5]]
<strong>Output:</strong> 1
<strong>Explanation: </strong>There is only one way to choose hats given the conditions. 
First person choose hat 3, Second person choose hat 4 and last one hat 5.</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> hats = [[3,5,1],[3,5]]
<strong>Output:</strong> 4
<strong>Explanation: </strong>There are 4 ways to choose hats
(3,5), (5,3), (1,3) and (1,5)
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> hats = [[1,2,3,4],[1,2,3,4],[1,2,3,4],[1,2,3,4]]
<strong>Output:</strong> 24
<strong>Explanation: </strong>Each person can choose hats labeled from 1 to 4.
Number of Permutations of (1,2,3,4) = 24.
</pre>

<p><strong>Example 4:</strong></p>

<pre><strong>Input:</strong> hats = [[1,2,3],[2,3,5,6],[1,3,7,9],[1,8,9],[2,5,7]]
<strong>Output:</strong> 111
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>n == hats.length</code></li>
	<li><code>1 &lt;= n &lt;= 10</code></li>
	<li><code>1 &lt;= hats[i].length &lt;= 40</code></li>
	<li><code>1 &lt;= hats[i][j] &lt;= 40</code></li>
	<li><code>hats[i]</code> contains a list of <strong>unique</strong> integers.</li>
</ul>

**Companies**:  
[MindTickle](https://leetcode.com/company/mindtickle)

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/), [Bit Manipulation](https://leetcode.com/tag/bit-manipulation/), [Bitmask](https://leetcode.com/tag/bitmask/)

**Similar Questions**:
* [The Number of Good Subsets (Hard)](https://leetcode.com/problems/the-number-of-good-subsets/)

## Solution 1. DP + Bitmasking

>TLE 

```cpp
// OJ: https://leetcode.com/problems/number-of-ways-to-wear-different-hats-to-each-other/
// Author: A M A N
// Time : O(P * 2^H * H)
// Space: O(P * 2^H)
class Solution {
public:
    static const int M = 1e9+7;
    unordered_map<int, unordered_map<int64_t, int>> dp;
    bool IsBitSet(int64_t num, int bit){
        return 1LL == ((num >> bit) & 1LL);
    }
    int setBit(int64_t n, int k){
        return (n | (1LL << (k)));
    }
    int clearBit(int64_t n, int k){
        return (n & (~(1LL << (k))));
    }
    int solve(vector<vector<int>>&hats, int idx, int64_t vis){
        if(idx>=hats.size()) return 1;
        if(dp.find(idx)!=dp.end() and dp[idx].find(vis)!=dp[idx].end())
            return dp[idx][vis];
        int64_t ans = 0;
        for(int i=0; i<hats[idx].size(); i++){
            int hatNumber = hats[idx][i];
            if(!IsBitSet(vis, hatNumber)){
                vis = setBit(vis, hatNumber);
                (ans += solve(hats, idx+1, vis))%=M;
                vis = clearBit(vis, hatNumber);
            }
        }
        return dp[idx][vis] = ans%M;
    }
    int numberWays(vector<vector<int>>& hats) {
        return solve(hats, 0, 0);
    }
};
```

## Solution 2. DP + Bitmasking

```cpp
// OJ: https://leetcode.com/problems/number-of-ways-to-wear-different-hats-to-each-other/
// Author: A M A N
// Time : O(H * 2^P * P) where H is no. of Hats, and P is no. of People
// Space: O(H * 2^P)
class Solution {
public:
    const static int M = 1e9 + 7;
    int FINISH;
    unordered_map<int, unordered_map<int64_t, int>>dp;
    vector<vector<int>> hatToPeople;

    int solve(int idx, int64_t state){
        if(state == FINISH)
            return 1;
        if(idx > 40)
            return 0;
        if(dp.find(idx)!=dp.end() and dp[idx].find(state)!=dp[idx].end())
            return dp[idx][state];
        int res = solve(idx+1, state); // skip this hat
        for(auto people: hatToPeople[idx]){
            if(((state>>people) & 1LL) == 1LL) continue; // cur people is already wearing a hat
            (res+= solve(idx+1, state | (1LL<<people)))%=M;
        }
        return dp[idx][state] = res%M;       
    }

    int numberWays(vector<vector<int>>& hats) {
        int noOfPeople = hats.size();
        hatToPeople.resize(41, vector<int>()); 
        for(int i=0; i<hats.size(); i++){
            for(auto hat: hats[i])
                hatToPeople[hat].push_back(i);
        }
        FINISH = (1<<noOfPeople)-1; // finish state (all people have an assigned hat)
        return solve(0, 0);
    }
};
```