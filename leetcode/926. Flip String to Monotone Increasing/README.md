# [926. Flip String to Monotone Increasing (Medium)](https://leetcode.com/problems/flip-string-to-monotone-increasing/)

<p>A binary string is monotone increasing if it consists of some number of <code>0</code>'s (possibly none), followed by some number of <code>1</code>'s (also possibly none).</p>

<p>You are given a binary string <code>s</code>. You can flip <code>s[i]</code> changing it from <code>0</code> to <code>1</code> or from <code>1</code> to <code>0</code>.</p>

<p>Return <em>the minimum number of flips to make </em><code>s</code><em> monotone increasing</em>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> s = "00110"
<strong>Output:</strong> 1
<strong>Explanation:</strong> We flip the last digit to get 00111.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> s = "010110"
<strong>Output:</strong> 2
<strong>Explanation:</strong> We flip to get 011111, or alternatively 000111.
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> s = "00011000"
<strong>Output:</strong> 2
<strong>Explanation:</strong> We flip to get 00000000.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s[i]</code> is either <code>'0'</code> or <code>'1'</code>.</li>
</ul>


**Related Topics**:  
[String](https://leetcode.com/tag/string/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/)

## Solution 1.
 
```cpp
// OJ: https://leetcode.com/problems/flip-string-to-monotone-increasing/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    int minFlipsMonoIncr(string s) {
        int n = s.size();
        vector<int> z(n), o(n);
        // cost to flip s[i...i] to zero
        z[0] = (s[0]=='1');
        for(int i=1; i<n; i++)
            z[i] = z[i-1] + (s[i]=='1');
        //cost to flip s[i...n-1] to one
        o[n-1] = (s[n-1]=='0');
        for(int i=n-2; i>=0; i--)
            o[i] = o[i+1] + (s[i]=='0');
        int ans = INT_MAX;
        for(int i=0; i<=n; i++)
            ans = min(ans, (i?z[i-1]:0)+(i==n?0:o[i]));
        return ans;
    }
};
```