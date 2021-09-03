# [616. Add Bold Tag in String (Medium)](https://leetcode.com/problems/add-bold-tag-in-string/)

<p>You are given a string <code>s</code> and an array of strings <code>words</code>. You should add a closed pair of bold tag <code>&lt;b&gt;</code> and <code>&lt;/b&gt;</code> to wrap the substrings in <code>s</code> that exist in <code>words</code>. If two such substrings overlap, you should wrap them together with only one pair of closed bold-tag. If two substrings wrapped by bold tags are consecutive, you should combine them.</p>

<p>Return <code>s</code> <em>after adding the bold tags</em>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> s = "abcxyz123", words = ["abc","123"]
<strong>Output:</strong> "&lt;b&gt;abc&lt;/b&gt;xyz&lt;b&gt;123&lt;/b&gt;"
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> s = "aaabbcc", words = ["aaa","aab","bc"]
<strong>Output:</strong> "&lt;b&gt;aaabbc&lt;/b&gt;c"
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 1000</code></li>
	<li><code>0 &lt;= words.length &lt;= 100</code></li>
	<li><code>1 &lt;= words[i].length &lt;= 1000</code></li>
	<li><code>s</code> and <code>words[i]</code> consist of English letters and digits.</li>
	<li>All the values of <code>words</code> are <strong>unique</strong>.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Note:</strong> This question is the same as 758: <a href="https://leetcode.com/problems/bold-words-in-string/" target="_blank">https://leetcode.com/problems/bold-words-in-string/</a></p>


**Companies**:  
[Facebook](https://leetcode.com/company/facebook)

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Hash Table](https://leetcode.com/tag/hash-table/), [String](https://leetcode.com/tag/string/), [Trie](https://leetcode.com/tag/trie/), [String Matching](https://leetcode.com/tag/string-matching/)

**Similar Questions**:
* [Merge Intervals (Medium)](https://leetcode.com/problems/merge-intervals/)
* [Tag Validator (Hard)](https://leetcode.com/problems/tag-validator/)

## Solution 1.

```cpp
// OJ: https://leetcode.com/problems/add-bold-tag-in-string/
// Author: A M A N
// Time : O(N*M*K) where K is the length of the longest word in dict
// Space: O(N)
};
class Solution {
public:
    string addBoldTag(string s, vector<string>& words) {
        unordered_map<int, int>bold;
        for(auto w: words){ 
            int l = w.size();
            for(int i=0; i+l-1<s.size(); i++){ 
                if(w==s.substr(i, l))
                    for(int j=i; j<i+l; j++)
                        bold[j]=1;
            }
        }
        string ans;
        int bolded = false;
        for(int i=0; i<s.size(); i++){
            if(!bolded and bold[i]){
                ans+="<b>";
                bolded = true;
            }else if(bolded and !bold[i]){
                ans+="</b>";
                bolded = false;
            }
            ans+=s[i];
        }
        if(bolded)
            ans+="</b>";
        return ans;
    }
};
```