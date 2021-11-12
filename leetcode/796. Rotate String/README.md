# [796. Rotate String (Easy)](https://leetcode.com/problems/rotate-string/)

<p>Given two strings <code>s</code> and <code>goal</code>, return <code>true</code> <em>if and only if</em> <code>s</code> <em>can become</em> <code>goal</code> <em>after some number of <strong>shifts</strong> on</em> <code>s</code>.</p>

<p>A <strong>shift</strong> on <code>s</code> consists of moving the leftmost character of <code>s</code> to the rightmost position.</p>

<ul>
	<li>For example, if <code>s = "abcde"</code>, then it will be <code>"bcdea"</code> after one shift.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<pre><strong>Input:</strong> s = "abcde", goal = "cdeab"
<strong>Output:</strong> true
</pre><p><strong>Example 2:</strong></p>
<pre><strong>Input:</strong> s = "abcde", goal = "abced"
<strong>Output:</strong> false
</pre>
<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= s.length, goal.length &lt;= 100</code></li>
	<li><code>s</code> and <code>goal</code> consist of lowercase English letters.</li>
</ul>


**Companies**:  
[Amazon](https://leetcode.com/company/amazon), [Zoom](https://leetcode.com/company/zoom)

**Related Topics**:  
[String](https://leetcode.com/tag/string/), [String Matching](https://leetcode.com/tag/string-matching/)

## Solution 1. Brute-force

```cpp
// OJ: https://leetcode.com/problems/rotate-string/
// Author: A M A N
// Time : O(N^2)
// Space: O(N)
class Solution {
public:
    bool rotateString(string s, string goal) {
        for(int i=0; i<s.size(); i++){
            string a = s.substr(0, i);
            string b = s.substr(i);
            if(goal == b + a)
                return true;
        }
        return false;
    }
};
```