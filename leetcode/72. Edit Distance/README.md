# [72. Edit Distance (Hard)](https://leetcode.com/problems/edit-distance/)

<p>Given two strings <code>word1</code> and <code>word2</code>, return <em>the minimum number of operations required to convert <code>word1</code> to <code>word2</code></em>.</p>

<p>You have the following three operations permitted on a word:</p>

<ul>
	<li>Insert a character</li>
	<li>Delete a character</li>
	<li>Replace a character</li>
</ul>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> word1 = "horse", word2 = "ros"
<strong>Output:</strong> 3
<strong>Explanation:</strong> 
horse -&gt; rorse (replace 'h' with 'r')
rorse -&gt; rose (remove 'r')
rose -&gt; ros (remove 'e')
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> word1 = "intention", word2 = "execution"
<strong>Output:</strong> 5
<strong>Explanation:</strong> 
intention -&gt; inention (remove 't')
inention -&gt; enention (replace 'i' with 'e')
enention -&gt; exention (replace 'n' with 'x')
exention -&gt; exection (replace 'n' with 'c')
exection -&gt; execution (insert 'u')
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>0 &lt;= word1.length, word2.length &lt;= 500</code></li>
	<li><code>word1</code> and <code>word2</code> consist of lowercase English letters.</li>
</ul>


**Related Topics**:  
[String](https://leetcode.com/tag/string/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/)

**Similar Questions**:
* [One Edit Distance (Medium)](https://leetcode.com/problems/one-edit-distance/)
* [Delete Operation for Two Strings (Medium)](https://leetcode.com/problems/delete-operation-for-two-strings/)
* [Minimum ASCII Delete Sum for Two Strings (Medium)](https://leetcode.com/problems/minimum-ascii-delete-sum-for-two-strings/)
* [Uncrossed Lines (Medium)](https://leetcode.com/problems/uncrossed-lines/)

## Solution 1. DP

```cpp
// OJ: https://leetcode.com/problems/edit-distance/
// Author: A M A N
// Time : O(N*M)
// Space: O(N*M)
class Solution {
public:
    int minDistance(string a, string b) {
        int n = a.size();
        int m = b.size();
        vector<vector<int>> dp(n+1, vector<int>(m+1));
        //Base Case --Initialization--
        for(int i=0; i<=n; i++)dp[i][0] = i;        
        for(int i=0; i<=m; i++)dp[0][i] = i;
        
        for(int i=1; i<=n; i++){
            for(int j=1; j<=m; j++)
                if(a[i-1]==b[j-1])
                    dp[i][j] = dp[i-1][j-1];
                else
                    dp[i][j] = 1 + min({dp[i-1][j], dp[i][j-1], dp[i-1][j-1]});
        }
        return dp[n][m];
    }
};
```


## Solution 2. Recursion + Memoization

```cpp
// OJ: https://leetcode.com/problems/edit-distance/
// Author: A M A N
// Time : O(N*M)
// Space: O(N*M)
class Solution {
public:
    int dp[501][501];
    int edit(string &a, string &b, int n, int m){
        if(n==0) return m;
        if(m==0) return n;
        if(dp[n][m]!=-1)
            return dp[n][m];
        if(a[n-1]==b[m-1])
            return dp[n][m] = edit(a, b, n-1, m-1);
        else
            return dp[n][m] = 1 + min({edit(a, b, n, m-1), edit(a, b, n-1, m), edit(a, b, n-1, m-1)});
    }
    int minDistance(string a, string b) {
        memset(dp, -1, sizeof dp);
        return edit(a, b, a.size(), b.size());
    }
};
```