# [647. Palindromic Substrings (Medium)](https://leetcode.com/problems/palindromic-substrings/)

<p>Given a string <code>s</code>, return <em>the number of <strong>palindromic substrings</strong> in it</em>.</p>

<p>A string is a <strong>palindrome</strong> when it reads the same backward as forward.</p>

<p>A <strong>substring</strong> is a contiguous sequence of characters within the string.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> s = "abc"
<strong>Output:</strong> 3
<strong>Explanation:</strong> Three palindromic strings: "a", "b", "c".
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> s = "aaa"
<strong>Output:</strong> 6
<strong>Explanation:</strong> Six palindromic strings: "a", "a", "a", "aa", "aa", "aaa".
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 1000</code></li>
	<li><code>s</code> consists of lowercase English letters.</li>
</ul>


**Related Topics**:  
[String](https://leetcode.com/tag/string/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/)

**Similar Questions**:
* [Longest Palindromic Substring (Medium)](https://leetcode.com/problems/longest-palindromic-substring/)
* [Longest Palindromic Subsequence (Medium)](https://leetcode.com/problems/longest-palindromic-subsequence/)

## Solution 1.

> The idea is count the number of different palindromic substrings from their respective middle.

```cpp
// OJ: https://leetcode.com/problems/palindromic-substrings/
// Author: A M A N
// Time : O(N*N)
// Space: O(1)
class Solution {
public:
    int cnt;
    void findPalindromes(string &s, int l, int h){
        while(l>=0 and h<s.size() and s[l]==s[h]){ //expanding in both direction
            // sett.insert(s.substr(l, h-l+1));
            cnt++;
            l--, h++;
        }
    }
    int countSubstrings(string s) {
        for(int i=0; i<s.size(); i++){ 
            findPalindromes(s, i, i); // find palindromes of odd length, with s[i] as its midpoint
            findPalindromes(s, i, i+1); // find palindromes of even length, with s[i] and s[i+1] as its midpoints
        }
        return cnt;
    }
};
```