# [1573. Number of Ways to Split a String (Medium)](https://leetcode.com/problems/number-of-ways-to-split-a-string/)

<p>Given a binary string <code>s</code> (a string consisting only of '0's and '1's),&nbsp;we can split <code>s</code>&nbsp;into 3 <strong>non-empty</strong> strings s1, s2, s3 (s1+ s2+ s3 = s).</p>

<p>Return the number of ways <code>s</code> can be split such that the number of&nbsp;characters '1' is the same in s1, s2, and s3.</p>

<p>Since the answer&nbsp;may be too large,&nbsp;return it modulo&nbsp;10^9 + 7.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> s = "10101"
<strong>Output:</strong> 4
<strong>Explanation:</strong> There are four ways to split s in 3 parts where each part contain the same number of letters '1'.
"1|010|1"
"1|01|01"
"10|10|1"
"10|1|01"
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> s = "1001"
<strong>Output:</strong> 0
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> s = "0000"
<strong>Output:</strong> 3
<strong>Explanation:</strong> There are three ways to split s in 3 parts.
"0|0|00"
"0|00|0"
"00|0|0"
</pre>

<p><strong>Example 4:</strong></p>

<pre><strong>Input:</strong> s = "100100010100110"
<strong>Output:</strong> 12
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>3 &lt;= s.length &lt;= 10^5</code></li>
	<li><code>s[i]</code> is <code>'0'</code>&nbsp;or&nbsp;<code>'1'</code>.</li>
</ul>


**Companies**:  
[Microsoft](https://leetcode.com/company/microsoft)

**Related Topics**:  
[Math](https://leetcode.com/tag/math/), [String](https://leetcode.com/tag/string/)

**Similar Questions**:
* [Split Array with Equal Sum (Hard)](https://leetcode.com/problems/split-array-with-equal-sum/)

## Solution 1. Prefix Sum

```cpp
// OJ: https://leetcode.com/problems/number-of-ways-to-split-a-string/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    const int M = 1e9+7;
    int numWays(string s) {
        int n = s.size();
        vector<int>ones(n, 0);
        for(int i=0; i<n; i++) ones[i] = (s[i]-'0') + (i ? ones[i-1]:0); 
        if(ones[n-1]%3!=0) return 0; // can't split the string into 3 parts with equal no of 1s.
        int k = ones[n-1]/3; 
        if(ones[n-1]==0) // if there's no 1 in the string
            return (1LL*(n-1)*(n-2)/2)%M; // pick any 2 splits out of n-1 splits (n-1)C(2)
        int a = upper_bound(ones.begin(), ones.end(), k)   - lower_bound(ones.begin(), ones.end(), k);
        int b = upper_bound(ones.begin(), ones.end(), 2*k) - lower_bound(ones.begin(), ones.end(), 2*k);
        return (1LL*a*b)%M;
    }
};
```