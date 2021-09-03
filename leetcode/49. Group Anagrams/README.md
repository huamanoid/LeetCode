# [49. Group Anagrams (Medium)](https://leetcode.com/problems/group-anagrams/)

<p>Given an array of strings <code>strs</code>, group <strong>the anagrams</strong> together. You can return the answer in <strong>any order</strong>.</p>

<p>An <strong>Anagram</strong> is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<pre><strong>Input:</strong> strs = ["eat","tea","tan","ate","nat","bat"]
<strong>Output:</strong> [["bat"],["nat","tan"],["ate","eat","tea"]]
</pre><p><strong>Example 2:</strong></p>
<pre><strong>Input:</strong> strs = [""]
<strong>Output:</strong> [[""]]
</pre><p><strong>Example 3:</strong></p>
<pre><strong>Input:</strong> strs = ["a"]
<strong>Output:</strong> [["a"]]
</pre>
<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= strs.length &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= strs[i].length &lt;= 100</code></li>
	<li><code>strs[i]</code> consists of lower-case English letters.</li>
</ul>


**Related Topics**:  
[Hash Table](https://leetcode.com/tag/hash-table/), [String](https://leetcode.com/tag/string/), [Sorting](https://leetcode.com/tag/sorting/)

**Similar Questions**:
* [Valid Anagram (Easy)](https://leetcode.com/problems/valid-anagram/)
* [Group Shifted Strings (Medium)](https://leetcode.com/problems/group-shifted-strings/)

## Solution 1. Sorting

```cpp
// OJ: https://leetcode.com/problems/group-anagrams/
// Author: A M A N
// Time : O(N*MlogM)
// Space: O(N)
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string, vector<string>>mp;
        for(auto &s: strs){
            string ss = s;
            sort(ss.begin(), ss.end());
            mp[ss].push_back(s);
        }
        vector<vector<string>> ans;
        for(auto a: mp)
            ans.push_back(a.second);
        return ans;
    }
};
```