# [1140. Stone Game II (Medium)](https://leetcode.com/problems/stone-game-ii/)

<p>Alice and Bob continue their&nbsp;games with piles of stones.&nbsp; There are a number of&nbsp;piles&nbsp;<strong>arranged in a row</strong>, and each pile has a positive integer number of stones&nbsp;<code>piles[i]</code>.&nbsp; The objective of the game is to end with the most&nbsp;stones.&nbsp;</p>

<p>Alice&nbsp;and Bob take turns, with Alice starting first.&nbsp; Initially, <code>M = 1</code>.</p>

<p>On each player's turn, that player&nbsp;can take <strong>all the stones</strong> in the <strong>first</strong> <code>X</code> remaining piles, where <code>1 &lt;= X &lt;= 2M</code>.&nbsp; Then, we set&nbsp;<code>M = max(M, X)</code>.</p>

<p>The game continues until all the stones have been taken.</p>

<p>Assuming Alice and Bob play optimally, return the maximum number of stones Alice&nbsp;can get.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> piles = [2,7,9,4,4]
<strong>Output:</strong> 10
<strong>Explanation:</strong>  If Alice takes one pile at the beginning, Bob takes two piles, then Alice takes 2 piles again. Alice can get 2 + 4 + 4 = 10 piles in total. If Alice takes two piles at the beginning, then Bob can take all three piles left. In this case, Alice get 2 + 7 = 9 piles in total. So we return 10 since it's larger. 
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> piles = [1,2,3,4,5,100]
<strong>Output:</strong> 104
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= piles.length &lt;= 100</code></li>
	<li><code>1 &lt;= piles[i]&nbsp;&lt;= 10<sup>4</sup></code></li>
</ul>


**Companies**:  
[Google](https://leetcode.com/company/google), [Microsoft](https://leetcode.com/company/microsoft), [Bloomberg](https://leetcode.com/company/bloomberg)

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Math](https://leetcode.com/tag/math/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/), [Game Theory](https://leetcode.com/tag/game-theory/)

**Similar Questions**:
* [Stone Game V (Hard)](https://leetcode.com/problems/stone-game-v/)
* [Stone Game VI (Medium)](https://leetcode.com/problems/stone-game-vi/)
* [Stone Game VII (Medium)](https://leetcode.com/problems/stone-game-vii/)
* [Stone Game VIII (Hard)](https://leetcode.com/problems/stone-game-viii/)

## Solution 1. Recursion + Memoization

The problem is similar to [Stone Game III](https://leetcode.com/problems/stone-game-iii/)

```cpp
// OJ: https://leetcode.com/problems/stone-game-ii/
// Author: A M A N
// Time : O(N^2)
// Space: O(N^2)
class Solution {
public:
    int dp[101][101];
    int solve(vector<int>&piles, int start, int M){
        if(start>=piles.size())
            return 0;
        //maximum stones the current player can get from piles[i:] with M 
        if(dp[start][M]!=-1)
            return dp[start][M];
        int sum = 0;
        int ans = INT_MIN;
        for(int i=start; i<min(start+2*M, (int)piles.size()); i++){
            sum+=piles[i];
            ans = max(ans, sum-solve(piles, i+1, max(i-start+1, M)));
        }
        return dp[start][M] = ans;
    }
    int stoneGameII(vector<int>& piles) {
        memset(dp, -1, sizeof dp);
        int offsetScore = solve(piles, 0, 1); // A - B;
        int S = accumulate(piles.begin(), piles.end(), 0); // A + B
        return (offsetScore + S)/2; 
    }
};
```