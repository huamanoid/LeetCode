# [940. Distinct Subsequences II (Hard)](https://leetcode.com/problems/distinct-subsequences-ii/)

<p>Given a string s, return <em>the number of <strong>distinct non-empty subsequences</strong> of</em> <code>s</code>. Since the answer may be very large, return it <strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>
A <strong>subsequence</strong> of a string is a new string that is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (i.e., <code>"ace"</code> is a subsequence of <code>"<u>a</u>b<u>c</u>d<u>e</u>"</code> while <code>"aec"</code> is not.
<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> s = "abc"
<strong>Output:</strong> 7
<strong>Explanation:</strong> The 7 distinct subsequences are "a", "b", "c", "ab", "ac", "bc", and "abc".
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> s = "aba"
<strong>Output:</strong> 6
<strong>Explanation:</strong> The 6 distinct subsequences are "a", "b", "ab", "aa", "ba", and "aba".
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> s = "aaa"
<strong>Output:</strong> 3
<strong>Explanation:</strong> The 3 distinct subsequences are "a", "aa" and "aaa".
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 2000</code></li>
	<li><code>s</code> consists of lowercase English letters.</li>
</ul>


**Companies**:  
[Google](https://leetcode.com/company/google)

**Related Topics**:  
[String](https://leetcode.com/tag/string/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/)

**Similar Questions**:
* [Number of Unique Good Subsequences (Hard)](https://leetcode.com/problems/number-of-unique-good-subsequences/)

## Solution 1. Recursion + Memoization

```cpp
// OJ: https://leetcode.com/problems/distinct-subsequences-ii/
// Author: A M A N
// Time : O(N^2)
// Space: O(N)
class Solution {
public:
    int dp[20001];
    const static int M = 1e9+7;
    int solve(string &s, int idx){
        if(idx>=s.size())return 0;
        if(dp[idx]!=-1)
            return dp[idx];
        int ans = 0;
        vector<int> vis(26, 0); // only count the first occurence of the char to avoid repeated subsequences
        for(int i=idx; i<s.size(); i++){
            if(vis[s[i]-'a']) continue;
            vis[s[i]-'a'] = 1;
            (ans+=1+solve(s, i+1))%=M;
        }
        return dp[idx] = ans;
    }
    int distinctSubseqII(string s) {
        memset(dp, -1, sizeof dp);
        return solve(s, 0);
    }
};
```