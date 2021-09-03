# [131. Palindrome Partitioning (Medium)](https://leetcode.com/problems/palindrome-partitioning/)

<p>Given a string <code>s</code>, partition <code>s</code> such that every substring of the partition is a <strong>palindrome</strong>. Return all possible palindrome partitioning of <code>s</code>.</p>

<p>A <strong>palindrome</strong> string is a string that reads the same backward as forward.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<pre><strong>Input:</strong> s = "aab"
<strong>Output:</strong> [["a","a","b"],["aa","b"]]
</pre><p><strong>Example 2:</strong></p>
<pre><strong>Input:</strong> s = "a"
<strong>Output:</strong> [["a"]]
</pre>
<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 16</code></li>
	<li><code>s</code> contains only lowercase English letters.</li>
</ul>


**Related Topics**:  
[String](https://leetcode.com/tag/string/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/), [Backtracking](https://leetcode.com/tag/backtracking/)

**Similar Questions**:
* [Palindrome Partitioning II (Hard)](https://leetcode.com/problems/palindrome-partitioning-ii/)
* [Palindrome Partitioning IV (Hard)](https://leetcode.com/problems/palindrome-partitioning-iv/)

## Solution 1. Backtracking

```cpp
// OJ: https://leetcode.com/problems/palindrome-partitioning/
// Author: A M A N
//Time:  O(N * 2^N)
//Space: O(N)
class Solution {
public:

    vector<vector<string>> ans;
    bool isPalindrome(string s){
        string ss = s;
        reverse(s.begin(), s.end());
        return ss == s;
    }
    void solve(string s, vector<string> &temp){
        if(s.size()==0){
            ans.push_back(temp);
            return;
        }
        for(int i = 1; i<=s.size(); i++){
            string left  = s.substr(0, i);
            string right = s.substr(i);
            if(isPalindrome(left)){
                temp.push_back(left);
                solve(right, temp);
                temp.pop_back();
            }    
        }
        return; 
    }
    vector<vector<string>> partition(string s) {
        vector<string> temp;
        solve(s, temp);
        return ans;
    }
};
```