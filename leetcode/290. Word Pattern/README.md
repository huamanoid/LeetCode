# [290. Word Pattern (Easy)](https://leetcode.com/problems/word-pattern/)

<p>Given a <code>pattern</code> and a string <code>s</code>, find if <code>s</code>&nbsp;follows the same pattern.</p>

<p>Here <b>follow</b> means a full match, such that there is a bijection between a letter in <code>pattern</code> and a <b>non-empty</b> word in <code>s</code>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> pattern = "abba", s = "dog cat cat dog"
<strong>Output:</strong> true
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> pattern = "abba", s = "dog cat cat fish"
<strong>Output:</strong> false
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> pattern = "aaaa", s = "dog cat cat dog"
<strong>Output:</strong> false
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= pattern.length &lt;= 300</code></li>
	<li><code>pattern</code> contains only lower-case English letters.</li>
	<li><code>1 &lt;= s.length &lt;= 3000</code></li>
	<li><code>s</code> contains only lowercase English letters and spaces <code>' '</code>.</li>
	<li><code>s</code> <strong>does not contain</strong> any leading or trailing spaces.</li>
	<li>All the words in <code>s</code> are separated by a <strong>single space</strong>.</li>
</ul>


**Companies**:  
[Amazon](https://leetcode.com/company/amazon), [Bolt](https://leetcode.com/company/bolt)

**Related Topics**:  
[Hash Table](https://leetcode.com/tag/hash-table/), [String](https://leetcode.com/tag/string/)

**Similar Questions**:
* [Isomorphic Strings (Easy)](https://leetcode.com/problems/isomorphic-strings/)
* [Word Pattern II (Medium)](https://leetcode.com/problems/word-pattern-ii/)

## Solution 1. Hashing

```cpp
// OJ: https://leetcode.com/problems/word-pattern/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    bool wordPattern(string pattern, string s) {
        stringstream ss(s);
        string word="";
        map<char, string> mp;
        map<string, char> revMap;
        int idx = 0;
        int noOfWords = 0;
        while(ss >> word){
            noOfWords++;
            if(mp.find(pattern[idx])==mp.end()){
                if(revMap.find(word)!=revMap.end()){
                    if(revMap[word]!=pattern[idx])
                        return false;
                }
                mp[pattern[idx]] = word;
                revMap[word] = pattern[idx];
            }else{
                if(mp[pattern[idx]]!=word){
                    return false;
                }
            }
            idx++;
        }
        return (noOfWords==pattern.size());
    }
};
```