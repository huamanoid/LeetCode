# [567. Permutation in String (Medium)](https://leetcode.com/problems/permutation-in-string/)

<p>Given two strings <code>s1</code> and <code>s2</code>, return <code>true</code><em> if </em><code>s2</code><em> contains a permutation of </em><code>s1</code><em>, or </em><code>false</code><em> otherwise</em>.</p>

<p>In other words, return <code>true</code> if one of <code>s1</code>'s permutations is the substring of <code>s2</code>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> s1 = "ab", s2 = "eidbaooo"
<strong>Output:</strong> true
<strong>Explanation:</strong> s2 contains one permutation of s1 ("ba").
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> s1 = "ab", s2 = "eidboaoo"
<strong>Output:</strong> false
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= s1.length, s2.length &lt;= 10<sup>4</sup></code></li>
	<li><code>s1</code> and <code>s2</code> consist of lowercase English letters.</li>
</ul>


**Related Topics**:  
[Hash Table](https://leetcode.com/tag/hash-table/), [Two Pointers](https://leetcode.com/tag/two-pointers/), [String](https://leetcode.com/tag/string/), [Sliding Window](https://leetcode.com/tag/sliding-window/)

**Similar Questions**:
* [Minimum Window Substring (Hard)](https://leetcode.com/problems/minimum-window-substring/)
* [Find All Anagrams in a String (Medium)](https://leetcode.com/problems/find-all-anagrams-in-a-string/)

## Solution 1. Sliding Window

```cpp
// OJ: https://leetcode.com/problems/permutation-in-string/
// Author: A M A N
// Time : O(N)
// Space: O(1)
class Solution {
public:
    bool checkInclusion(string s1, string s2) {
        unordered_map<int, int> mp;
        for(auto c: s1)mp[c]++;
        int cnt = mp.size();
        int i=0, j=0;
        while(j<s2.size()){
            if(mp.find(s2[j])!=mp.end()){
                mp[s2[j]]--;
                if(mp[s2[j]]==0)cnt--;
            }
            int window = j-i+1;
            if(window<s1.size())
                j++;
            else if(window==s1.size()){
                if(cnt==0)return true;
                if(mp.find(s2[i])!=mp.end()){
                    mp[s2[i]]++;
                    if(mp[s2[i]]==1)cnt++;
                }
                i++, j++; // slide the window
            }
        }
        return false;
    }
};
```