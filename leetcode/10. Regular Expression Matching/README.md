# [10. Regular Expression Matching (Hard)](https://leetcode.com/problems/regular-expression-matching/)

<p>Given an input string <code>s</code>&nbsp;and a pattern <code>p</code>, implement regular expression matching with support for <code>'.'</code> and <code>'*'</code> where:</p>

<ul>
	<li><code>'.'</code> Matches any single character.​​​​</li>
	<li><code>'*'</code> Matches zero or more of the preceding element.</li>
</ul>

<p>The matching should cover the <strong>entire</strong> input string (not partial).</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> s = "aa", p = "a"
<strong>Output:</strong> false
<strong>Explanation:</strong> "a" does not match the entire string "aa".
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> s = "aa", p = "a*"
<strong>Output:</strong> true
<strong>Explanation:</strong>&nbsp;'*' means zero or more of the preceding&nbsp;element, 'a'. Therefore, by repeating 'a' once, it becomes "aa".
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> s = "ab", p = ".*"
<strong>Output:</strong> true
<strong>Explanation:</strong>&nbsp;".*" means "zero or more (*) of any character (.)".
</pre>

<p><strong>Example 4:</strong></p>

<pre><strong>Input:</strong> s = "aab", p = "c*a*b"
<strong>Output:</strong> true
<strong>Explanation:</strong>&nbsp;c can be repeated 0 times, a can be repeated 1 time. Therefore, it matches "aab".
</pre>

<p><strong>Example 5:</strong></p>

<pre><strong>Input:</strong> s = "mississippi", p = "mis*is*p*."
<strong>Output:</strong> false
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= s.length&nbsp;&lt;= 20</code></li>
	<li><code>1 &lt;= p.length&nbsp;&lt;= 30</code></li>
	<li><code>s</code> contains only lowercase English letters.</li>
	<li><code>p</code> contains only lowercase English letters, <code>'.'</code>, and&nbsp;<code>'*'</code>.</li>
	<li>It is guaranteed for each appearance of the character <code>'*'</code>, there will be a previous valid character to match.</li>
</ul>


**Related Topics**:  
[String](https://leetcode.com/tag/string/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/), [Recursion](https://leetcode.com/tag/recursion/)

**Similar Questions**:
* [Wildcard Matching (Hard)](https://leetcode.com/problems/wildcard-matching/)

## Solution 1. DP

```cpp
// OJ: https://leetcode.com/problems/regular-expression-matching/
// Author: A M A N
// Time : O(N * M)
// Space: O(N * M)
class Solution {
public:
    bool isMatch(string A, string B) {
        int n = A.size();
        int m = B.size();
        vector<vector<bool>> dp(n+1, vector<bool>(m+1, false));
        // Initialization
        for(int i=0;i<=n;i++)  dp[i][0] = false;
        dp[0][0] = true;
        for(int j=1;j<=m;j++)  
            if(B[j-1]=='*')
                dp[0][j] = dp[0][j-2];
            else
                dp[0][j] = false;
                
        for(int i=1;i<=n;i++){
            for(int j=1;j<=m;j++){
                if(A[i-1]==B[j-1] or B[j-1]=='.')
                    dp[i][j] = dp[i-1][j-1];
                else if(B[j-1]=='*'){
                        if(A[i-1]==B[j-2] or B[j-2]=='.')
                            dp[i][j] = dp[i][j-2] or dp[i-1][j];
                        else
                            dp[i][j] = dp[i][j-2];
                }
                else
                    dp[i][j] = false;
            }
        }
        return dp[n][m];
    }
};
```