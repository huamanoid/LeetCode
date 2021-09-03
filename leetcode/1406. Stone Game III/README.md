# [1406. Stone Game III (Hard)](https://leetcode.com/problems/stone-game-iii/)

<p>Alice and Bob continue their&nbsp;games with piles of stones. There are several stones&nbsp;<strong>arranged in a row</strong>, and each stone has an associated&nbsp;value which is an integer given in the array&nbsp;<code>stoneValue</code>.</p>

<p>Alice and Bob take turns, with <strong>Alice</strong> starting first. On each player's turn, that player&nbsp;can take <strong>1, 2 or 3 stones</strong>&nbsp;from&nbsp;the <strong>first</strong> remaining stones in the row.</p>

<p>The score of each player is the sum of values of the stones taken. The score of each player is <strong>0</strong>&nbsp;initially.</p>

<p>The objective of the game is to end with the highest score, and the winner is the player with the highest score and there could be a tie. The game continues until all the stones have been taken.</p>

<p>Assume&nbsp;Alice&nbsp;and Bob&nbsp;<strong>play optimally</strong>.</p>

<p>Return <em>"Alice"</em> if&nbsp;Alice will win, <em>"Bob"</em> if Bob will win or <em>"Tie"</em> if they end the game with the same score.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> values = [1,2,3,7]
<strong>Output:</strong> "Bob"
<strong>Explanation:</strong> Alice will always lose. Her best move will be to take three piles and the score become 6. Now the score of Bob is 7 and Bob wins.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> values = [1,2,3,-9]
<strong>Output:</strong> "Alice"
<strong>Explanation:</strong> Alice must choose all the three piles at the first move to win and leave Bob with negative score.
If Alice chooses one pile her score will be 1 and the next move Bob's score becomes 5. The next move Alice will take the pile with value = -9 and lose.
If Alice chooses two piles her score will be 3 and the next move Bob's score becomes 3. The next move Alice will take the pile with value = -9 and also lose.
Remember that both play optimally so here Alice will choose the scenario that makes her win.
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> values = [1,2,3,6]
<strong>Output:</strong> "Tie"
<strong>Explanation:</strong> Alice cannot win this game. She can end the game in a draw if she decided to choose all the first three piles, otherwise she will lose.
</pre>

<p><strong>Example 4:</strong></p>

<pre><strong>Input:</strong> values = [1,2,3,-1,-2,-3,7]
<strong>Output:</strong> "Alice"
</pre>

<p><strong>Example 5:</strong></p>

<pre><strong>Input:</strong> values = [-1,-2,-3]
<strong>Output:</strong> "Tie"
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= values.length &lt;= 50000</code></li>
	<li><code>-1000&nbsp;&lt;= values[i] &lt;= 1000</code></li>
</ul>

**Companies**:  
[Google](https://leetcode.com/company/google)

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Math](https://leetcode.com/tag/math/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/), [Game Theory](https://leetcode.com/tag/game-theory/)

**Similar Questions**:
* [Stone Game V (Hard)](https://leetcode.com/problems/stone-game-v/)
* [Stone Game VI (Medium)](https://leetcode.com/problems/stone-game-vi/)
* [Stone Game VII (Medium)](https://leetcode.com/problems/stone-game-vii/)
* [Stone Game VIII (Hard)](https://leetcode.com/problems/stone-game-viii/)

## Solution 1.

>TLE

```cpp
// OJ: https://leetcode.com/problems/stone-game-iii/
// Author: A M A N
// Time : O()
// Space: O()
};
class Solution {
public:
    //Alice = 1;
    //Tie = 2;
    //Bob = 3;
    map<array<int, 4>, int> mp;
    int solve(vector<int>&nums, int n, int A, int B, bool turnA){
        if(n < 0){
            if(A>B)
                return 1;
            else if(A==B)
                return 2;
            else
                return 3;
        }
        if(mp.find({n, A, B, turnA})!=mp.end())
            return mp[{n, A, B, turnA}];
        if(turnA){
            int one = 1e9, two = 1e9, three = 1e9;
            one = solve(nums, n-1, A+nums[n], B, 0);
            if(n-1>=0)
                two = solve(nums, n-2, A+nums[n]+nums[n-1], B, 0);
            if(n-2>=0)
                three = solve(nums, n-3, A+nums[n]+nums[n-1]+nums[n-2], B, 0);
            return mp[{n, A, B, turnA}] = min({one, two, three}); // minimizing for Alice
        }else{
            int one = -1e9, two = -1e9, three = -1e9;
            one = solve(nums, n-1, A, B+nums[n], 1);
            if(n-1>=0)
                two = solve(nums, n-2, A, B+nums[n]+nums[n-1], 1);
            if(n-2>=0)
                three = solve(nums, n-3, A, B+nums[n]+nums[n-1]+nums[n-2], 1);
            return mp[{n, A, B, turnA}] = max({one, two, three}); // maximizing for Bob
        }
    }
    string stoneGameIII(vector<int>& nums) {
        reverse(nums.begin(), nums.end());
        int ans = solve(nums, nums.size()-1, 0, 0, 1);
        if(ans==1) return "Alice";
        else if(ans==2) return "Tie";
        else return "Bob";
        return "";
    }
};
```

## Solution 2. Recursion + Memoization

```cpp
// OJ: https://leetcode.com/problems/stone-game-iii/
// Author: A M A N
// Time : O(N)
// Space: O(N)
};
class Solution {
public:
    vector<int> dp;
    int playOptimally(vector<int>&stones, int start){
        if(start>=stones.size())
            return 0;
        // Let dp[i] be the best result the current player can get at this ith element.
        if(dp[start]!=INT_MIN)
            return dp[start];
        int sum = 0; 
        int ans = INT_MIN;
        for(int i=start; i<min(start+3, (int)stones.size()); i++){ // picking 1, 2, and 3 stones & best(max) of all
            sum+= stones[i];
            // maximizing the current score for each player
            ans = max(ans, sum - playOptimally(stones, i+1)); // current player score = current score - playOptimally(i+1);
        }
        return dp[start] = ans;
    }
    string stoneGameIII(vector<int>& stones) {
        dp.assign(stones.size()+1, INT_MIN);
        int score = playOptimally(stones, 0); //score = TotalScore(A) - TotalScore(B);
        if(score>0) return "Alice";
        else if(score<0) return "Bob";
        else return "Tie";
    }
};
```


