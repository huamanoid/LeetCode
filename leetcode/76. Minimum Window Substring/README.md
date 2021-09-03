# [76. Minimum Window Substring (Hard)](https://leetcode.com/problems/minimum-window-substring/)

<p>Given two strings <code>s</code> and <code>t</code> of lengths <code>m</code> and <code>n</code> respectively, return <em>the <strong>minimum window substring</strong> of </em><code>s</code><em> such that every character in </em><code>t</code><em> (<strong>including duplicates</strong>) is included in the window. If there is no such substring</em><em>, return the empty string </em><code>""</code><em>.</em></p>

<p>The testcases will be generated such that the answer is <strong>unique</strong>.</p>

<p>A <strong>substring</strong> is a contiguous sequence of characters within the string.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> s = "ADOBECODEBANC", t = "ABC"
<strong>Output:</strong> "BANC"
<strong>Explanation:</strong> The minimum window substring "BANC" includes 'A', 'B', and 'C' from string t.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> s = "a", t = "a"
<strong>Output:</strong> "a"
<strong>Explanation:</strong> The entire string s is the minimum window.
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> s = "a", t = "aa"
<strong>Output:</strong> ""
<strong>Explanation:</strong> Both 'a's from t must be included in the window.
Since the largest window of s only has one 'a', return empty string.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>m == s.length</code></li>
	<li><code>n == t.length</code></li>
	<li><code>1 &lt;= m, n&nbsp;&lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> and <code>t</code> consist of uppercase and lowercase English letters.</li>
</ul>

<p>&nbsp;</p>
<strong>Follow up:</strong> Could you find an algorithm that runs in <code>O(m + n)</code> time?

**Related Topics**:  
[Hash Table](https://leetcode.com/tag/hash-table/), [String](https://leetcode.com/tag/string/), [Sliding Window](https://leetcode.com/tag/sliding-window/)

**Similar Questions**:
* [Substring with Concatenation of All Words (Hard)](https://leetcode.com/problems/substring-with-concatenation-of-all-words/)
* [Minimum Size Subarray Sum (Medium)](https://leetcode.com/problems/minimum-size-subarray-sum/)
* [Sliding Window Maximum (Hard)](https://leetcode.com/problems/sliding-window-maximum/)
* [Permutation in String (Medium)](https://leetcode.com/problems/permutation-in-string/)
* [Smallest Range Covering Elements from K Lists (Hard)](https://leetcode.com/problems/smallest-range-covering-elements-from-k-lists/)
* [Minimum Window Subsequence (Hard)](https://leetcode.com/problems/minimum-window-subsequence/)

## Solution 1. Sliding Window

```cpp
// OJ: https://leetcode.com/problems/minimum-window-substring/
// Author: A M A N
// Time : O(N)
// Space: O(UniqueChars(t))
class Solution {
public:
    string minWindow(string s, string t) {
        int minLength = INT_MAX;
        string ans;
        // since the order of the string 't' does not matter, Only the chars and their frequencies so we use map
        unordered_map<char, int> mp;
        for(char c: t) mp[c]++;
        // map tells us how many chars with their freq do we need at the moment to span t
        // cnt saves us time to check whether each value in the map is 0 or not
        int cnt = mp.size(); // no of unique chars in pattern
        int i=0, j=0;   // i=> start of window, j=> end of window
        while(j<s.size()){
            if(mp.find(s[j])!=mp.end()){
                mp[s[j]]--; // included s[j] in the window, we decrement it coz we need one less occurence of s[j]
                if(mp[s[j]]==0) // means we have included all the required occurences of s[j] in the window 
                    cnt--;  // so now we need one less character to include, when cnt = 0, it means all chars are included
            }             
            while(cnt==0){  // while the conditions are met, try decreasing the window length
                int windowLength = j-i+1;
                if(minLength>windowLength){
                    minLength = windowLength;
                    ans = s.substr(i, windowLength);
                }
                if(mp.find(s[i])!=mp.end()){ // char at start of the window is a 't' char
                    mp[s[i]]++; // we exclude this char, as we decrease the window length, so we need one more occurence of it
                    if(mp[s[i]]==1)cnt++; // means it was 0 before, so we need one more character to include 
                }
                i++; // increment the start of window, decrease the window length
            }
            j++; // extend the window
        }
        return ans;
    }
};
```