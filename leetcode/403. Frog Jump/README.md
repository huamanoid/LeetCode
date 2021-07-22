# [403. Frog Jump (Hard)](https://leetcode.com/problems/frog-jump/)

<p>A frog is crossing a river. The river is divided into some number of units, and at each unit, there may or may not exist a stone. The frog can jump on a stone, but it must not jump into the water.</p>

<p>Given a list of <code>stones</code>' positions (in units) in sorted <strong>ascending order</strong>, determine if the frog can cross the river by landing on the last stone. Initially, the frog is on the first stone and assumes the first jump must be <code>1</code> unit.</p>

<p>If the frog's last jump was <code>k</code> units, its next jump must be either <code>k - 1</code>, <code>k</code>, or <code>k + 1</code> units. The frog can only jump in the forward direction.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> stones = [0,1,3,5,6,8,12,17]
<strong>Output:</strong> true
<strong>Explanation:</strong> The frog can jump to the last stone by jumping 1 unit to the 2nd stone, then 2 units to the 3rd stone, then 2 units to the 4th stone, then 3 units to the 6th stone, 4 units to the 7th stone, and 5 units to the 8th stone.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> stones = [0,1,2,3,4,8,9,11]
<strong>Output:</strong> false
<strong>Explanation:</strong> There is no way to jump to the last stone as the gap between the 5th and 6th stone is too large.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>2 &lt;= stones.length &lt;= 2000</code></li>
	<li><code>0 &lt;= stones[i] &lt;= 2<sup>31</sup> - 1</code></li>
	<li><code>stones[0] == 0</code></li>
	<li><code>stones</code>&nbsp;is sorted in a strictly increasing order.</li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/)

**Similar Questions**:
* [Minimum Sideway Jumps (Medium)](https://leetcode.com/problems/minimum-sideway-jumps/)

## Solution 1. Recursion + Memoization

```cpp
// OJ: https://leetcode.com/problems/frog-jump/
// Author: A M A N
// Time : O(N^2 * logN)
// Space: O(N^2)
class Solution {
public:
    int end;
    unordered_set<int> sett;
    map<pair<int, int>, bool> dp;
    bool solve(int pos, int k){
        if(!sett.count(pos))  return false;
        if(pos == end)        return true;
        
        if(dp.find({pos, k})!=dp.end())
            return dp[{pos, k}];

        bool a = k>1 ? solve(pos+k-1, k-1) : false; 
        bool b = solve(pos+k,   k);
        bool c = pos!=0 ? solve(pos+k+1, k+1) : false; // k can only be 1 at the beginning

        return dp[{pos, k}] = a or b or c;
    }
    bool canCross(vector<int>& stones) {
        end = stones[stones.size()-1];
        for(auto i: stones) sett.insert(i);
        return solve(0, 1);
    }
};
```


## Solution 2. DP

```cpp
// OJ: https://leetcode.com/problems/frog-jump/
// Author: A M A N
// Time : O(N * N)
// Space: O(N * N)
class Solution {
public:
    bool canCross(vector<int>& stones) {
        int end = stones[stones.size()-1];
        unordered_map<int, unordered_set<int>> options;
        for(auto stone: stones) 
            options[stone] = unordered_set<int>{}; 
        options[0].insert(1);
        for(auto stone: stones){
            for(auto k: options[stone]){
                int next = stone + k;
                if(next==end)
                    return true;
                if(options.find(next)!=options.end()){
                    if(k>1) options[next].insert(k-1);
                    options[next].insert(k);
                    options[next].insert(k+1);
                }
            }
        }
        return false;
    }
};
```