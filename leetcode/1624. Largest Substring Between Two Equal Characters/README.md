# [1624. Largest Substring Between Two Equal Characters (Easy)](https://leetcode.com/problems/largest-substring-between-two-equal-characters/)

<p>Given a string <code>s</code>, return <em>the length of the longest substring between two equal characters, excluding the two characters.</em> If there is no such substring return <code>-1</code>.</p>

<p>A <strong>substring</strong> is a contiguous sequence of characters within a string.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> s = "aa"
<strong>Output:</strong> 0
<strong>Explanation:</strong> The optimal substring here is an empty substring between the two <code>'a's</code>.</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> s = "abca"
<strong>Output:</strong> 2
<strong>Explanation:</strong> The optimal substring here is "bc".
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> s = "cbzxy"
<strong>Output:</strong> -1
<strong>Explanation:</strong> There are no characters that appear twice in s.
</pre>

<p><strong>Example 4:</strong></p>

<pre><strong>Input:</strong> s = "cabbac"
<strong>Output:</strong> 4
<strong>Explanation:</strong> The optimal substring here is "abba". Other non-optimal substrings include "bb" and "".
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 300</code></li>
	<li><code>s</code> contains only lowercase English letters.</li>
</ul>


**Related Topics**:  
[Hash Table](https://leetcode.com/tag/hash-table/), [String](https://leetcode.com/tag/string/)

## Solution 1.

```cpp
// OJ: https://leetcode.com/problems/largest-substring-between-two-equal-characters/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    int maxLengthBetweenEqualCharacters(string s) {
        unordered_map<char, int> left, right;
        for(int i=0; i<s.size(); i++)
            if(left.find(s[i])==left.end())
                left[s[i]] = i;
        for(int i=s.size()-1; i>=0; i--)
            if(right.find(s[i])==right.end())
                right[s[i]] = i;
        int ans = -1;
        for(char c: s)
            ans = max(ans, right[c]-left[c]-1);
        return ans;
    }
};
```