# [887. Super Egg Drop (Hard)](https://leetcode.com/problems/super-egg-drop/)

<p>You are given <code>k</code> identical eggs and you have access to a building with <code>n</code> floors labeled from <code>1</code> to <code>n</code>.</p>

<p>You know that there exists a floor <code>f</code> where <code>0 &lt;= f &lt;= n</code> such that any egg dropped at a floor <strong>higher</strong> than <code>f</code> will <strong>break</strong>, and any egg dropped <strong>at or below</strong> floor <code>f</code> will <strong>not break</strong>.</p>

<p>Each move, you may take an unbroken egg and drop it from any floor <code>x</code> (where <code>1 &lt;= x &lt;= n</code>). If the egg breaks, you can no longer use it. However, if the egg does not break, you may <strong>reuse</strong> it in future moves.</p>

<p>Return <em>the <strong>minimum number of moves</strong> that you need to determine <strong>with certainty</strong> what the value of </em><code>f</code> is.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> k = 1, n = 2
<strong>Output:</strong> 2
<strong>Explanation: </strong>
Drop the egg from floor 1. If it breaks, we know that f = 0.
Otherwise, drop the egg from floor 2. If it breaks, we know that f = 1.
If it does not break, then we know f = 2.
Hence, we need at minimum 2 moves to determine with certainty what the value of f is.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> k = 2, n = 6
<strong>Output:</strong> 3
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> k = 3, n = 14
<strong>Output:</strong> 4
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= 100</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>4</sup></code></li>
</ul>


**Related Topics**:  
[Math](https://leetcode.com/tag/math/), [Binary Search](https://leetcode.com/tag/binary-search/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/)

**Similar Questions**:
* [Egg Drop With 2 Eggs and N Floors (Medium)](https://leetcode.com/problems/egg-drop-with-2-eggs-and-n-floors/)



## Solution 1. Recursion + Memoization

`TLE`
```cpp
// OJ: https://leetcode.com/problems/super-egg-drop/
// Author: A M A N
// Time : O(N^2 * K)
// Space: O(N * K)
class Solution {
public:
    unordered_map<int, unordered_map<int, int>> dp;
    int superEggDrop(int e, int f) {
        if(e==1 or f<=1) return f;
        if(dp.find(e)!=dp.end() and dp[e].find(f)!=dp[e].end())
            return dp[e][f];
        int ans = INT_MAX;
        for(int k=1; k<=f; k++){
            //egg breaks, e-1 remains, and remaining bottom floors to explore 
            //doesn't break, above floors to explore
            int temp = 1 + max(superEggDrop(e-1, k-1),superEggDrop(e, f-k));  // worst of both cases + 1 effort
            ans = min(ans, temp);
        }
        return dp[e][f] = ans;
    }
};
```


> \+ Binary Search inplace of linear search

```cpp
// OJ: https://leetcode.com/problems/super-egg-drop/
// Author: A M A N
// Time : O(N * LogN * K)
// Space: O(N * K)
class Solution {
public:
    unordered_map<int, unordered_map<int, int>> dp;
    int superEggDrop(int e, int f) {
        if(e==1 or f<=1) return f;
        if(dp.find(e)!=dp.end() and dp[e].find(f)!=dp[e].end())
            return dp[e][f];
        int ans = INT_MAX;
        int low = 1, high = f;
        while(low<=high){
            int mid = low + (high-low)/2;
            int up   = superEggDrop(e, f - mid); //egg doesn't break, above floors to explore
            int down = superEggDrop(e-1, mid-1); //egg breaks, e-1 remains, and remaining bottom floors to explore
            //moving in the direction of higher effort to lookout for worst case
            if(up > down)
                low = mid+1; 
            else
                high = mid-1;

            ans = min(ans, 1+max(up,down));
        }
        return dp[e][f] = ans;
    }
};
```