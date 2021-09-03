# [557. Reverse Words in a String III (Easy)](https://leetcode.com/problems/reverse-words-in-a-string-iii/)

<p>Given a string <code>s</code>, reverse the order of characters in each word within a sentence while still preserving whitespace and initial word order.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<pre><strong>Input:</strong> s = "Let's take LeetCode contest"
<strong>Output:</strong> "s'teL ekat edoCteeL tsetnoc"
</pre><p><strong>Example 2:</strong></p>
<pre><strong>Input:</strong> s = "God Ding"
<strong>Output:</strong> "doG gniD"
</pre>
<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>s</code> contains printable <strong>ASCII</strong> characters.</li>
	<li><code>s</code> does not contain any leading or trailing spaces.</li>
	<li>There is <strong>at least one</strong> word in <code>s</code>.</li>
	<li>All the words in <code>s</code> are separated by a single space.</li>
</ul>


**Companies**:  
[Apple](https://leetcode.com/company/apple), [Paypal](https://leetcode.com/company/paypal), [eBay](https://leetcode.com/company/ebay)

**Related Topics**:  
[Two Pointers](https://leetcode.com/tag/two-pointers/), [String](https://leetcode.com/tag/string/)

**Similar Questions**:
* [Reverse String II (Easy)](https://leetcode.com/problems/reverse-string-ii/)

## Solution 1.

```cpp
// OJ: https://leetcode.com/problems/reverse-words-in-a-string-iii/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    vector<string> breakSentence(string& s){
        vector<string> ans;
        int i=0;
        while(i<s.size()){
            int j = i;
            while(j<s.size() and s[j]!=' ')j++;
            ans.push_back(s.substr(i, j-i));
            i = j+1;
        }
        return ans;
    }
    string reverseWords(string s) {
        vector<string> words = breakSentence(s);
        string ans;
        for(auto s: words){
            reverse(s.begin(), s.end());
            ans+=s;
            ans+=' ';
        }
        return ans.substr(0, ans.size()-1);
    }
};
```