# [132. Palindrome Partitioning II (Hard)](https://leetcode.com/problems/palindrome-partitioning-ii/)

<p>Given a string <code>s</code>, partition <code>s</code> such that every substring of the partition is a palindrome.</p>

<p>Return <em>the minimum cuts needed</em> for a palindrome partitioning of <code>s</code>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> s = "aab"
<strong>Output:</strong> 1
<strong>Explanation:</strong> The palindrome partitioning ["aa","b"] could be produced using 1 cut.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> s = "a"
<strong>Output:</strong> 0
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> s = "ab"
<strong>Output:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 2000</code></li>
	<li><code>s</code> consists of lower-case English letters only.</li>
</ul>


**Related Topics**:  
[String](https://leetcode.com/tag/string/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/)

**Similar Questions**:
* [Palindrome Partitioning (Medium)](https://leetcode.com/problems/palindrome-partitioning/)
* [Palindrome Partitioning IV (Hard)](https://leetcode.com/problems/palindrome-partitioning-iv/)

## Solution 1. Recursive DP (Divide & Conquer)

```cpp
// OJ: https://leetcode.com/problems/palindrome-partitioning-ii/
// Author: A M A N
// Time : O(N^3)
// Space: O(N^2)
class Solution {
public:
//     another optimization could be building a palindrome dp matrix in N^2 time
    bool isPalindrome(string &s, int i, int j){ // passing string with start and end pointer is faster than passing the substring, as substring-ing is itself linear not constant
        if(i>=j) return true;
        if(s[i]!=s[j]) return false;
        return isPalindrome(s, i+1, j-1);
    }
    int dp[2001][2001];
    int solve(string s, int i, int j){
        if(i>=j)
            return 0;
        if(dp[i][j]!=-1)
            return dp[i][j];
        if(isPalindrome(s, i, j))
            return dp[i][j] = 0;
        int ans = 1e9;
        for(int k = i; k<=j-1; k++){
//             Optimization line: if the first part itself is not a palindrome then there's no point continuing with the right part
            if(!isPalindrome(s, i, k))
                continue;
            int l = solve(s, i, k);
            int r = solve(s, k+1, j);
            ans = min(ans, l+r+1);
        }
        return dp[i][j] = ans;
    }
    int minCut(string s) {
        memset(dp, -1, sizeof dp);
        int n = s.size();
        return solve(s, 0, n-1);
    }
};
```

Optimizing the run time by pre-computing isPalindrome matrix

```cpp
// OJ: https://leetcode.com/problems/palindrome-partitioning-ii/
// Author: A M A N
// Time : O(N^2)
// Space: O(N^2)
class Solution {
public:
    bool isPalindrome[2001][2001];
    void buildPalindrome(string &s, int i, int j){
        int n = s.size();
        for(int i = n-1; i>=0; i--)
            for(int j=i; j<n; j++){
                int len = j-i+1;
                if(len==1)
                    isPalindrome[i][j] = true;
                else if(len == 2)
                    isPalindrome[i][j] = s[i]==s[j];
                else
                    isPalindrome[i][j] = (s[i]==s[j]) and isPalindrome[i+1][j-1]; // checking the end positions and if middle is pal
                // since we need dp[i+1][j-1], we're traversing i from right and j from left
            }
    }
    int dp[2001][2001];
    int solve(string s, int i, int j){
        if(i>=j)
            return 0;
        if(dp[i][j]!=-1)
            return dp[i][j];
        if(isPalindrome[i][j])
            return dp[i][j] = 0;
        int ans = 1e9;
        for(int k = i; k<j; k++){
          // Optimization line: if the first part itself is not a palindrome then there's no point continuing with the right part
            if(!isPalindrome[i][k])
                continue;
            int l = solve(s, i, k);
            int r = solve(s, k+1, j);
            ans = min(ans, l+r+1);
        }
        return dp[i][j] = ans;
    }
    int minCut(string s) {
        memset(dp, -1, sizeof dp);
        buildPalindrome(s, 0, s.size()-1);
        return solve(s, 0, s.size()-1);
    }
};
```