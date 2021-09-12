# [890. Find and Replace Pattern (Medium)](https://leetcode.com/problems/find-and-replace-pattern/)

<p>Given a list of strings <code>words</code> and a string <code>pattern</code>, return <em>a list of</em> <code>words[i]</code> <em>that match</em> <code>pattern</code>. You may return the answer in <strong>any order</strong>.</p>

<p>A word matches the pattern if there exists a permutation of letters <code>p</code> so that after replacing every letter <code>x</code> in the pattern with <code>p(x)</code>, we get the desired word.</p>

<p>Recall that a permutation of letters is a bijection from letters to letters: every letter maps to another letter, and no two letters map to the same letter.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> words = ["abc","deq","mee","aqq","dkd","ccc"], pattern = "abb"
<strong>Output:</strong> ["mee","aqq"]
<strong>Explanation:</strong> "mee" matches the pattern because there is a permutation {a -&gt; m, b -&gt; e, ...}. 
"ccc" does not match the pattern because {a -&gt; c, b -&gt; c, ...} is not a permutation, since a and b map to the same letter.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> words = ["a","b","c"], pattern = "a"
<strong>Output:</strong> ["a","b","c"]
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= pattern.length &lt;= 20</code></li>
	<li><code>1 &lt;= words.length &lt;= 50</code></li>
	<li><code>words[i].length == pattern.length</code></li>
	<li><code>pattern</code> and <code>words[i]</code> are lowercase English letters.</li>
</ul>


**Companies**:  
[Apple](https://leetcode.com/company/apple)

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Hash Table](https://leetcode.com/tag/hash-table/), [String](https://leetcode.com/tag/string/)

## Solution 1. Hash Table

```cpp
// OJ: https://leetcode.com/problems/find-and-replace-pattern/
// Author: A M A N
// Time : O(N * L) where N is the no of words and L is the length of longest word
// Space: O(N * L)
class Solution {
public:
    vector<string> findAndReplacePattern(vector<string>& words, string p) {
        vector<string> ans;
        for(auto s: words){
            bool found = true;
            map<char, char> mp;
            set<char> mapped; // to take care of the bijection (one to one) mapping
            for(int i=0; i<p.size(); i++){
                if(mp.find(p[i])==mp.end()){
                    mp[p[i]] = s[i];
                    if(mapped.count(s[i])){
                        found  = false; 
                        break;
                    }
                    mapped.insert(s[i]);
                }else {
                    if(s[i]!=mp[p[i]]){
                        found = false;
                        break;
                    }
                }
            }
            if(found) ans.push_back(s);
        }
        return ans;
    }
};
```