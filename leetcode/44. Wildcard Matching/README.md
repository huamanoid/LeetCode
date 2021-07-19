# [44. Wildcard Matching (Hard)](https://leetcode.com/problems/wildcard-matching/)

<p>Given an input string (<code>s</code>) and a pattern (<code>p</code>), implement wildcard pattern matching with support for <code>'?'</code> and <code>'*'</code> where:</p>

<ul>
	<li><code>'?'</code> Matches any single character.</li>
	<li><code>'*'</code> Matches any sequence of characters (including the empty sequence).</li>
</ul>

<p>The matching should cover the <strong>entire</strong> input string (not partial).</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> s = "aa", p = "a"
<strong>Output:</strong> false
<strong>Explanation:</strong> "a" does not match the entire string "aa".
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> s = "aa", p = "*"
<strong>Output:</strong> true
<strong>Explanation:</strong>&nbsp;'*' matches any sequence.
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> s = "cb", p = "?a"
<strong>Output:</strong> false
<strong>Explanation:</strong>&nbsp;'?' matches 'c', but the second letter is 'a', which does not match 'b'.
</pre>

<p><strong>Example 4:</strong></p>

<pre><strong>Input:</strong> s = "adceb", p = "*a*b"
<strong>Output:</strong> true
<strong>Explanation:</strong>&nbsp;The first '*' matches the empty sequence, while the second '*' matches the substring "dce".
</pre>

<p><strong>Example 5:</strong></p>

<pre><strong>Input:</strong> s = "acdcb", p = "a*c?b"
<strong>Output:</strong> false
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>0 &lt;= s.length, p.length &lt;= 2000</code></li>
	<li><code>s</code> contains only lowercase English letters.</li>
	<li><code>p</code> contains only lowercase English letters, <code>'?'</code> or <code>'*'</code>.</li>
</ul>


**Related Topics**:  
[String](https://leetcode.com/tag/string/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/), [Greedy](https://leetcode.com/tag/greedy/), [Recursion](https://leetcode.com/tag/recursion/)

**Similar Questions**:
* [Regular Expression Matching (Hard)](https://leetcode.com/problems/regular-expression-matching/)

## Solution 1. DP

```cpp
// OJ: https://leetcode.com/problems/wildcard-matching/
// Author: A M A N
// Time : O(N*M)
// Space: O(N*M)
class Solution {
public:
    bool isMatch(string A, string B) {
        int n = A.size();
        int m = B.size();
        vector<vector<bool>> dp(n+1, vector<bool>(m+1, false));
        // Initialization
        for(int i=0;i<=n;i++)  dp[i][0] = false;
        dp[0][0] = true;
        for(int j=0;j<=m;j++){
            dp[0][j] = true;
            // In order to be true, string B must have all '*' before it because A is an empty string
            for(int k=1;k<=j;k++)
                if(B[k-1]!='*'){
                    dp[0][j] = false;
                    break;
                }
        }
        for(int i=1;i<=n;i++){
            for(int j=1;j<=m;j++){
                if(A[i-1]==B[j-1] or B[j-1]=='?')
                    dp[i][j] = dp[i-1][j-1];
                else if(B[j-1]=='*')
                    dp[i][j] = dp[i-1][j] or dp[i][j-1];
                else
                    dp[i][j] = false;
            }
        }
        return dp[n][m];
    }
};
```