# [877. Stone Game (Medium)](https://leetcode.com/problems/stone-game/)

<p>Alex and Lee play a game with piles of stones.&nbsp; There are an even number of&nbsp;piles <strong>arranged in a row</strong>, and each pile has a positive integer number of stones <code>piles[i]</code>.</p>

<p>The objective of the game is to end with the most&nbsp;stones.&nbsp; The total number of stones is odd, so there are no ties.</p>

<p>Alex and Lee take turns, with Alex starting first.&nbsp; Each turn, a player&nbsp;takes the entire pile of stones from either the beginning or the end of the row.&nbsp; This continues until there are no more piles left, at which point the person with the most stones wins.</p>

<p>Assuming Alex and Lee play optimally, return <code>True</code>&nbsp;if and only if Alex wins the game.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> piles = [5,3,4,5]
<strong>Output:</strong> true
<strong>Explanation: </strong>
Alex starts first, and can only take the first 5 or the last 5.
Say he takes the first 5, so that the row becomes [3, 4, 5].
If Lee takes 3, then the board is [4, 5], and Alex takes 5 to win with 10 points.
If Lee takes the last 5, then the board is [3, 4], and Alex takes 4 to win with 9 points.
This demonstrated that taking the first 5 was a winning move for Alex, so we return true.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>2 &lt;= piles.length &lt;= 500</code></li>
	<li><code>piles.length</code> is even.</li>
	<li><code>1 &lt;= piles[i] &lt;= 500</code></li>
	<li><code>sum(piles)</code> is odd.</li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Math](https://leetcode.com/tag/math/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/), [Game Theory](https://leetcode.com/tag/game-theory/)

**Similar Questions**:
* [Stone Game V (Hard)](https://leetcode.com/problems/stone-game-v/)
* [Stone Game VI (Medium)](https://leetcode.com/problems/stone-game-vi/)
* [Stone Game VII (Medium)](https://leetcode.com/problems/stone-game-vii/)
* [Stone Game VIII (Hard)](https://leetcode.com/problems/stone-game-viii/)

## Solution 1. Recursive DP 

```cpp
// OJ: https://leetcode.com/problems/stone-game/
// Author: A M A N
// Time : O(N^2)
// Space: O(N^2)
class Solution {
public:
    int dp[2][501][501];
    bool solve(vector<int>&piles, int turnA, int aSum, int bSum, int start, int end){
        if(start>end) 
            return aSum>bSum;
        if(dp[turnA][start][end]!=-1)
            return dp[turnA][start][end];
        if(turnA){
            int first = solve(piles, 0, aSum+piles[start], bSum, start+1, end);
            int last  = solve(piles, 0, aSum+piles[end], bSum, start, end-1);
            return dp[turnA][start][end] = first or last;
        }else {
            int first = solve(piles, 1, aSum, bSum+piles[start], start+1, end);
            int last  = solve(piles, 1, aSum, bSum+piles[end],   start,   end-1);
            return dp[turnA][start][end] = first or last;
        }
    }
    bool stoneGame(vector<int>& piles) {
        memset(dp, -1, sizeof dp);
        return solve(piles, 1, 0, 0, 0, piles.size()-1);
    }
};
```