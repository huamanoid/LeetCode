# [1745. Palindrome Partitioning IV (Hard)](https://leetcode.com/problems/palindrome-partitioning-iv/)

<p>Given a string <code>s</code>, return <code>true</code> <em>if it is possible to split the string</em> <code>s</code> <em>into three <strong>non-empty</strong> palindromic substrings. Otherwise, return </em><code>false</code>.​​​​​</p>

<p>A string is said to be palindrome if it the same string when reversed.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> s = "abcbdd"
<strong>Output:</strong> true
<strong>Explanation: </strong>"abcbdd" = "a" + "bcb" + "dd", and all three substrings are palindromes.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> s = "bcbddxy"
<strong>Output:</strong> false
<strong>Explanation: </strong>s cannot be split into 3 palindromes.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>3 &lt;= s.length &lt;= 2000</code></li>
	<li><code>s</code>​​​​​​ consists only of lowercase English letters.</li>
</ul>


**Related Topics**:  
[String](https://leetcode.com/tag/string/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/)

**Similar Questions**:
* [Palindrome Partitioning (Medium)](https://leetcode.com/problems/palindrome-partitioning/)
* [Palindrome Partitioning II (Hard)](https://leetcode.com/problems/palindrome-partitioning-ii/)
* [Palindrome Partitioning III (Hard)](https://leetcode.com/problems/palindrome-partitioning-iii/)

## Solution 1. DP

```cpp
// OJ: https://leetcode.com/problems/palindrome-partitioning-iv/
// Author: A M A N
// Time : O(N^2)
// Space: O(N^2)
class Solution {
public:
    bool solve(string s, int i, int j){
        int n = s.length();
        vector<vector<bool>> dp(n, vector<bool>(n, false));
        /* dp[i][j] is true if str[i...j] is palindrome */
        for(int i = n - 1; i >= 0; i--) {
            for(int j = i; j < n; j++) {
                int len = j - i + 1;
                if(len == 1) {
                    dp[i][j] = true;
                }
                else if(len == 2) {
                    dp[i][j] = (s[i] == s[j]);
                }
                else {
                    dp[i][j] = (s[i] == s[j]) and (dp[i + 1][j - 1]); // since we need dp[i+1][j-1], we're traversing i from right and j from left
                }
            }
        }
        for(int k = i; k<=j-2; k++){
            for(int q = k+1; q<=j-1; q++){
             // basically checking 
             // if(isPalindrome(s, i, k) and isPalindrome(s, k+1, q) and isPalindrome(s, q+1, j))
                if(dp[i][k] and dp[k+1][q] and dp[q+1][j])
                    return true;
            }
        }
        return false;
    }
    bool checkPartitioning(string s) {
        int n = s.size();
        return solve(s, 0, n-1);
    }
};
```