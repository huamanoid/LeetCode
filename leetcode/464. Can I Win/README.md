# [464. Can I Win (Medium)](https://leetcode.com/problems/can-i-win/)

<p>In the "100 game" two players take turns adding, to a running total, any integer from <code>1</code> to <code>10</code>. The player who first causes the running total to <strong>reach or exceed</strong> 100 wins.</p>

<p>What if we change the game so that players <strong>cannot</strong> re-use integers?</p>

<p>For example, two players might take turns drawing from a common pool of numbers from 1 to 15 without replacement until they reach a total &gt;= 100.</p>

<p>Given two integers <code>maxChoosableInteger</code> and <code>desiredTotal</code>, return <code>true</code> if the first player to move can force a win, otherwise, return <code>false</code>. Assume both players play <strong>optimally</strong>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> maxChoosableInteger = 10, desiredTotal = 11
<strong>Output:</strong> false
<strong>Explanation:</strong>
No matter which integer the first player choose, the first player will lose.
The first player can choose an integer from 1 up to 10.
If the first player choose 1, the second player can only choose integers from 2 up to 10.
The second player will win by choosing 10 and get a total = 11, which is &gt;= desiredTotal.
Same with other integers chosen by the first player, the second player will always win.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> maxChoosableInteger = 10, desiredTotal = 0
<strong>Output:</strong> true
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> maxChoosableInteger = 10, desiredTotal = 1
<strong>Output:</strong> true
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= maxChoosableInteger &lt;= 20</code></li>
	<li><code>0 &lt;= desiredTotal &lt;= 300</code></li>
</ul>


**Companies**:  
[LinkedIn](https://leetcode.com/company/linkedin)

**Related Topics**:  
[Math](https://leetcode.com/tag/math/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/), [Bit Manipulation](https://leetcode.com/tag/bit-manipulation/), [Memoization](https://leetcode.com/tag/memoization/), [Game Theory](https://leetcode.com/tag/game-theory/), [Bitmask](https://leetcode.com/tag/bitmask/)

**Similar Questions**:
* [Flip Game II (Medium)](https://leetcode.com/problems/flip-game-ii/)
* [Guess Number Higher or Lower II (Medium)](https://leetcode.com/problems/guess-number-higher-or-lower-ii/)
* [Predict the Winner (Medium)](https://leetcode.com/problems/predict-the-winner/)

## Solution 1. 


It may seem that the required remaining `total` is the only DP state.
But we can have the same `total` by choosing different set of numbers.
which means the dp state should represent the current selected and remaining set.

Here in this implementation, the `vis` array itself is the DP state.  

>TLE

even after using `vector hashing`  

```cpp
// OJ: https://leetcode.com/problems/can-i-win/
// Author: A M A N
// Time : O()
// Space: O()
class Solution {
private:
    struct VectorHasher {
      size_t operator()(const vector<int>& V) const {
        size_t hash = V.size();
        for (auto i : V)
          hash ^= i + 0x9e3779b9 + (hash << 6) + (hash >> 2);
        return hash;
      }
    };

    int maxInt;
    unordered_map<vector<int>, int, VectorHasher> dp;
    
    bool solve(int total, vector<int>&vis) {
        if(total<=0) return m[vis] = false;
        
        if (dp.find(vis) != dp.end()) return dp[vis];
        bool canWin = false;
        for (int i = 1; i <= maxInt && !canWin; ++i) {
            if(vis[i])continue;
            vis[i] = 1;
            canWin = !solve(total - i, vis);
            vis[i] = 0;
        }
        return dp[vis] = canWin;
    }
public:
    bool canIWin(int maxChoosableInteger, int desiredTotal) {
        if (desiredTotal <= 0) return true;
        if (desiredTotal > (1 + maxChoosableInteger) * maxChoosableInteger / 2) return false;
        maxInt = maxChoosableInteger;
        vector<int>vis(21, false);
        return solve(desiredTotal, vis);
    }
};
```

## Solution 2. Recursion with Bitmask 

```cpp
// OJ: https://leetcode.com/problems/can-i-win/
// Author: A M A N
// Time : O()
// Space: O()
class Solution {
private:
    int maxInt;
    unsigned used = 0;
    map<unsigned, bool> dp;
    bool rec(int total) {
        if(total<=0) return dp[used] = false; 
        if (dp.find(used) != dp.end()) return dp[used];
        bool canWin = false;
        for (int i = 1; i <= maxInt && !canWin; ++i) {
            unsigned mask = (1 << i);
            if (!(used & mask)) {
                used |= mask;
                canWin = !rec(total - i);
                used &= ~mask;
            }
        }
        return dp[used] = canWin;
    }
public:
    bool canIWin(int maxChoosableInteger, int desiredTotal) {
        if (desiredTotal <= 0) return true;
        if (desiredTotal > (1 + maxChoosableInteger) * maxChoosableInteger / 2) return false;
        maxInt = maxChoosableInteger;
        return rec(desiredTotal);
    }
};
```