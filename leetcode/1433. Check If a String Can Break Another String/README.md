# [1433. Check If a String Can Break Another String (Medium)](https://leetcode.com/problems/check-if-a-string-can-break-another-string/)

<p>Given two strings: <code>s1</code> and <code>s2</code> with the same&nbsp;size, check if some&nbsp;permutation of string <code>s1</code> can break&nbsp;some&nbsp;permutation of string <code>s2</code> or vice-versa. In other words <code>s2</code> can break <code>s1</code>&nbsp;or vice-versa.</p>

<p>A string <code>x</code>&nbsp;can break&nbsp;string <code>y</code>&nbsp;(both of size <code>n</code>) if <code>x[i] &gt;= y[i]</code>&nbsp;(in alphabetical order)&nbsp;for all <code>i</code>&nbsp;between <code>0</code> and <code>n-1</code>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> s1 = "abc", s2 = "xya"
<strong>Output:</strong> true
<strong>Explanation:</strong> "ayx" is a permutation of s2="xya" which can break to string "abc" which is a permutation of s1="abc".
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> s1 = "abe", s2 = "acd"
<strong>Output:</strong> false 
<strong>Explanation:</strong> All permutations for s1="abe" are: "abe", "aeb", "bae", "bea", "eab" and "eba" and all permutation for s2="acd" are: "acd", "adc", "cad", "cda", "dac" and "dca". However, there is not any permutation from s1 which can break some permutation from s2 and vice-versa.
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> s1 = "leetcodee", s2 = "interview"
<strong>Output:</strong> true
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>s1.length == n</code></li>
	<li><code>s2.length == n</code></li>
	<li><code>1 &lt;= n &lt;= 10^5</code></li>
	<li>All strings consist of lowercase English letters.</li>
</ul>


**Companies**:  
[endurance](https://leetcode.com/company/endurance)

**Related Topics**:  
[String](https://leetcode.com/tag/string/), [Greedy](https://leetcode.com/tag/greedy/), [Sorting](https://leetcode.com/tag/sorting/)

## Solution 1.

```cpp
// OJ: https://leetcode.com/problems/check-if-a-string-can-break-another-string/
// Author: A M A N
// Time : O(NlogN)
// Space: O(1)
class Solution {
public:
    bool checkIfCanBreak(string s1, string s2) {
        sort(s1.begin(), s1.end());
        sort(s2.begin(), s2.end());
        bool ans1 = true, ans2 = true;
        for(int i=0; i<s1.size(); i++)
            if(s1[i]>s2[i])ans1 = false; 
            else if(s2[i]>s1[i]) ans2 = false;
        return ans1 or ans2; // (s1 can break s2) or (s2 can break s1) 
    }
};
```