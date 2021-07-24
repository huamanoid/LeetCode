# [1312. Minimum Insertion Steps to Make a String Palindrome (Hard)](https://leetcode.com/problems/minimum-insertion-steps-to-make-a-string-palindrome/)

<p>Given a string <code>s</code>. In one step you can insert any character at any index of the string.</p>

<p>Return <em>the minimum number of steps</em> to make <code>s</code>&nbsp;palindrome.</p>

<p>A&nbsp;<b>Palindrome String</b>&nbsp;is one that reads the same backward as well as forward.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> s = "zzazz"
<strong>Output:</strong> 0
<strong>Explanation:</strong> The string "zzazz" is already palindrome we don't need any insertions.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> s = "mbadm"
<strong>Output:</strong> 2
<strong>Explanation:</strong> String can be "mbdadbm" or "mdbabdm".
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> s = "leetcode"
<strong>Output:</strong> 5
<strong>Explanation:</strong> Inserting 5 characters the string becomes "leetcodocteel".
</pre>

<p><strong>Example 4:</strong></p>

<pre><strong>Input:</strong> s = "g"
<strong>Output:</strong> 0
</pre>

<p><strong>Example 5:</strong></p>

<pre><strong>Input:</strong> s = "no"
<strong>Output:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 500</code></li>
	<li>All characters of <code>s</code>&nbsp;are lower case English letters.</li>
</ul>


**Related Topics**:  
[String](https://leetcode.com/tag/string/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/)

## Solution 1. Recursion + Memoization (LCS)

```cpp
// OJ: https://leetcode.com/problems/minimum-insertion-steps-to-make-a-string-palindrome/
// Author: A M A N
// Time : O(N^2)
// Space: O(N^2)
class Solution {
public:
    int dp[501][501];
    int LCS(const string &a, const string &b, int n, int m){ //Passing by reference is important to avoid TLE
        if(n==a.size() or m==b.size())  return 0;
        if(dp[n][m]!=-1)
            return dp[n][m];
        if(a[n]==b[m])
            return dp[n][m] = 1+LCS(a, b, n+1, m+1);
        return dp[n][m] = max(LCS(a, b, n+1, m), LCS(a, b, n, m+1));
    }
    int minInsertions(string s) {
        string ss(s); reverse(ss.begin(), ss.end());
        memset(dp, -1, sizeof dp);
        return s.size() - LCS(s, ss, 0, 0); 
    }
};
```

## Solution 2. DP (LCS)

```cpp
// OJ: https://leetcode.com/problems/minimum-insertion-steps-to-make-a-string-palindrome/
// Author: A M A N
// Time : O(N^2)
// Space: O(N^2)
class Solution {
public:
    int minInsertions(string s) {
        int n = s.size();
        string rs(s); reverse(rs.begin(), rs.end());
        vector<vector<int>> dp(n+1, vector<int>(n+1, 0));
        for(int i=1; i<=n; i++)
            for(int j=1; j<=n; j++)
                if(s[i-1] == rs[j-1])
                    dp[i][j] = 1 + dp[i-1][j-1];
                else
                    dp[i][j] = max(dp[i-1][j], dp[i][j-1]);
                    
        return n - dp[n][n]; // n - LCS(s, rev(s));
    }
};
```