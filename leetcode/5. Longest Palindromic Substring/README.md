# [5. Longest Palindromic Substring (Medium)](https://leetcode.com/problems/longest-palindromic-substring/)

<p>Given a string <code>s</code>, return&nbsp;<em>the longest palindromic substring</em> in <code>s</code>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> s = "babad"
<strong>Output:</strong> "bab"
<strong>Note:</strong> "aba" is also a valid answer.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> s = "cbbd"
<strong>Output:</strong> "bb"
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> s = "a"
<strong>Output:</strong> "a"
</pre>

<p><strong>Example 4:</strong></p>

<pre><strong>Input:</strong> s = "ac"
<strong>Output:</strong> "a"
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 1000</code></li>
	<li><code>s</code> consist of only digits and English letters.</li>
</ul>


**Companies**:  
[Amazon](https://leetcode.com/company/amazon), [Microsoft](https://leetcode.com/company/microsoft), [Adobe](https://leetcode.com/company/adobe), [Apple](https://leetcode.com/company/apple), [Wayfair](https://leetcode.com/company/wayfair), [Facebook](https://leetcode.com/company/facebook), [Goldman Sachs](https://leetcode.com/company/goldman-sachs), [Oracle](https://leetcode.com/company/oracle), [Yahoo](https://leetcode.com/company/yahoo), [eBay](https://leetcode.com/company/ebay), [Google](https://leetcode.com/company/google), [Docusign](https://leetcode.com/company/docusign), [Uber](https://leetcode.com/company/uber), [Walmart Labs](https://leetcode.com/company/walmart-labs), [Qualcomm](https://leetcode.com/company/qualcomm), [Zoho](https://leetcode.com/company/zoho), [HBO](https://leetcode.com/company/hbo), [Tesla](https://leetcode.com/company/tesla)

**Related Topics**:  
[String](https://leetcode.com/tag/string/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/)

**Similar Questions**:
* [Shortest Palindrome (Hard)](https://leetcode.com/problems/shortest-palindrome/)
* [Palindrome Permutation (Easy)](https://leetcode.com/problems/palindrome-permutation/)
* [Palindrome Pairs (Hard)](https://leetcode.com/problems/palindrome-pairs/)
* [Longest Palindromic Subsequence (Medium)](https://leetcode.com/problems/longest-palindromic-subsequence/)
* [Palindromic Substrings (Medium)](https://leetcode.com/problems/palindromic-substrings/)

## Solution 1. Dynamic Programming

```cpp
// OJ: https://leetcode.com/problems/longest-palindromic-substring/
// Author: A M A N
// Time : O(N^2)
// Space: O(N^2)
class Solution {
public:
    string longestPalindrome(string s) {
        int n = s.size();
        vector<vector<int>> dp(n, vector<int>(n, 0));
        int len = 0;
        int start =-1;
        for(int i=n-1; i>=0; i--){ // start of the substring
            for(int j=i; j<n; j++){ // end of the substring
                if(i==j)
                    dp[i][j] = 1;
                else if(s[i]==s[j] and (dp[i+1][j-1]!=0 or i+1>j-1)) //start+1 > end-1
                    dp[i][j] = 2+dp[i+1][j-1];
                //candidates for ans                    
                if(len<dp[i][j]){
                    len = dp[i][j];
                    start = i;
                }
            }
        }
        return s.substr(start, len);
    }
};
```