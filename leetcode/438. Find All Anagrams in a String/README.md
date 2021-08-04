# [438. Find All Anagrams in a String (Medium)](https://leetcode.com/problems/find-all-anagrams-in-a-string/)

<p>Given two strings <code>s</code> and <code>p</code>, return <em>an array of all the start indices of </em><code>p</code><em>'s anagrams in </em><code>s</code>. You may return the answer in <strong>any order</strong>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> s = "cbaebabacd", p = "abc"
<strong>Output:</strong> [0,6]
<strong>Explanation:</strong>
The substring with start index = 0 is "cba", which is an anagram of "abc".
The substring with start index = 6 is "bac", which is an anagram of "abc".
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> s = "abab", p = "ab"
<strong>Output:</strong> [0,1,2]
<strong>Explanation:</strong>
The substring with start index = 0 is "ab", which is an anagram of "ab".
The substring with start index = 1 is "ba", which is an anagram of "ab".
The substring with start index = 2 is "ab", which is an anagram of "ab".
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= s.length, p.length &lt;= 3 * 10<sup>4</sup></code></li>
	<li><code>s</code> and <code>p</code> consist of lowercase English letters.</li>
</ul>


**Related Topics**:  
[Hash Table](https://leetcode.com/tag/hash-table/), [String](https://leetcode.com/tag/string/), [Sliding Window](https://leetcode.com/tag/sliding-window/)

**Similar Questions**:
* [Valid Anagram (Easy)](https://leetcode.com/problems/valid-anagram/)
* [Permutation in String (Medium)](https://leetcode.com/problems/permutation-in-string/)

## Solution 1. Sliding Window

```cpp
// OJ: https://leetcode.com/problems/find-all-anagrams-in-a-string/
// Author: A M A N
// Time : O(N)
// Space: O(UniqueChars(pattern))
// Ref  : https://www.youtube.com/watch?v=MW4lJ8Y0xXk
class Solution {
public:
    vector<int> findAnagrams(string s, string ana) {
        vector<int> ans;
        unordered_map<char, int> mp;
        for(char c: ana) mp[c]++;
        int K = ana.size(); // window size 
        int cnt = mp.size(); // no of distinct chars in the pattern
        int i=0, j=0;
        while(j<s.size()){
            if(mp.find(s[j])!=mp.end()){
                mp[s[j]]--;
                if(mp[s[j]]==0)cnt--; // one char is entirely matching so decrement the count
            }
            int window = j-i+1;
            if(window<K)
                j++; // increase the size of the window
            else if(window==K){
                if(cnt==0) // all chars of pattern are entirely matching in the string 
                    ans.push_back(i);
                if(mp.find(s[i])!=mp.end()){ // char at the beginning of the window is part of the the anagram
                    mp[s[i]]++;
                    if(mp[s[i]]==1) cnt++;
                }
                i++, j++; // slide the window
            }
        }
        return ans;        
    }
};
```