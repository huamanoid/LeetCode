# [696. Count Binary Substrings (Easy)](https://leetcode.com/problems/count-binary-substrings/)

<p>Give a binary string <code>s</code>, return the number of non-empty substrings that have the same number of <code>0</code>'s and <code>1</code>'s, and all the <code>0</code>'s and all the <code>1</code>'s in these substrings are grouped consecutively.</p>

<p>Substrings that occur multiple times are counted the number of times they occur.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> s = "00110011"
<strong>Output:</strong> 6
<strong>Explanation:</strong> There are 6 substrings that have equal number of consecutive 1's and 0's: "0011", "01", "1100", "10", "0011", and "01".
Notice that some of these substrings repeat and are counted the number of times they occur.
Also, "00110011" is not a valid substring because all the 0's (and 1's) are not grouped together.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> s = "10101"
<strong>Output:</strong> 4
<strong>Explanation:</strong> There are 4 substrings: "10", "01", "10", "01" that have equal number of consecutive 1's and 0's.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s[i]</code> is either <code>'0'</code> or <code>'1'</code>.</li>
</ul>


**Companies**:  
[Amazon](https://leetcode.com/company/amazon), [JPMorgan](https://leetcode.com/company/jpmorgan), [Salesforce](https://leetcode.com/company/salesforce)

**Related Topics**:  
[Two Pointers](https://leetcode.com/tag/two-pointers/), [String](https://leetcode.com/tag/string/)

**Similar Questions**:
* [Encode and Decode Strings (Medium)](https://leetcode.com/problems/encode-and-decode-strings/)

## Solution 1. Two pointers

```cpp
// OJ: https://leetcode.com/problems/count-binary-substrings/
// Author: A M A N
// Time : O(N)
// Space: O(1)
class Solution {
public:
    int countBinarySubstrings(string s) {
        int cnt=0;
        for(int i=0; i<s.size(); ){
            int j=i, t1=0, t2=0;
            while(j<s.size() and s[i]==s[j])t1++, j++; // consecutive 1s/0s
            i=j;
            while(j<s.size() and s[i]==s[j])t2++, j++; // consecutive 0s/1s
            cnt += min(t1, t2);
        }
        return cnt;
    }
};
```