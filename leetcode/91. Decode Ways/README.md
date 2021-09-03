# [91. Decode Ways (Medium)](https://leetcode.com/problems/decode-ways/)

<p>A message containing letters from <code>A-Z</code> can be <strong>encoded</strong> into numbers using the following mapping:</p>

<pre>'A' -&gt; "1"
'B' -&gt; "2"
...
'Z' -&gt; "26"
</pre>

<p>To <strong>decode</strong> an encoded message, all the digits must be grouped then mapped back into letters using the reverse of the mapping above (there may be multiple ways). For example, <code>"11106"</code> can be mapped into:</p>

<ul>
	<li><code>"AAJF"</code> with the grouping <code>(1 1 10 6)</code></li>
	<li><code>"KJF"</code> with the grouping <code>(11 10 6)</code></li>
</ul>

<p>Note that the grouping <code>(1 11 06)</code> is invalid because <code>"06"</code> cannot be mapped into <code>'F'</code> since <code>"6"</code> is different from <code>"06"</code>.</p>

<p>Given a string <code>s</code> containing only digits, return <em>the <strong>number</strong> of ways to <strong>decode</strong> it</em>.</p>

<p>The answer is guaranteed to fit in a <strong>32-bit</strong> integer.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> s = "12"
<strong>Output:</strong> 2
<strong>Explanation:</strong> "12" could be decoded as "AB" (1 2) or "L" (12).
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> s = "226"
<strong>Output:</strong> 3
<strong>Explanation:</strong> "226" could be decoded as "BZ" (2 26), "VF" (22 6), or "BBF" (2 2 6).
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> s = "0"
<strong>Output:</strong> 0
<strong>Explanation:</strong> There is no character that is mapped to a number starting with 0.
The only valid mappings with 0 are 'J' -&gt; "10" and 'T' -&gt; "20", neither of which start with 0.
Hence, there are no valid ways to decode this since all digits need to be mapped.
</pre>

<p><strong>Example 4:</strong></p>

<pre><strong>Input:</strong> s = "06"
<strong>Output:</strong> 0
<strong>Explanation:</strong> "06" cannot be mapped to "F" because of the leading zero ("6" is different from "06").
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 100</code></li>
	<li><code>s</code> contains only digits and may contain leading zero(s).</li>
</ul>


**Related Topics**:  
[String](https://leetcode.com/tag/string/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/)

**Similar Questions**:
* [Decode Ways II (Hard)](https://leetcode.com/problems/decode-ways-ii/)
 
## Solution 1. Recursion + Memoization

`Choice Diagram` is `key` to these type of questions.
```cpp
// OJ: https://leetcode.com/problems/decode-ways/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    unordered_map<int, int> dp;
    int solve(string& s, int n){
        if(n==0 and s[n]=='0')return 0;
        if(n<=0) return 1;
        if(dp.find(n)!=dp.end())
            return dp[n];

        if(s[n]=='0')
            if(s[n-1]=='1' or s[n-1]=='2')
                return dp[n] = solve(s, n-2); 
            else 
                return dp[n] = 0;
        
        if(s[n-1]!='0')
            if((s[n-1]=='2' and s[n]<='6') or s[n-1]=='1') 
                return dp[n] = solve(s, n-2) + solve(s, n-1);
        
        return dp[n] = solve(s, n-1);
    }
    int numDecodings(string s) {
        return solve(s, s.size()-1);
    }
};
```
Iterative version of the above solution

## Solution 2. DP

```cpp
// OJ: https://leetcode.com/problems/decode-ways/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    int numDecodings(string s) {
        int n = s.size();
        int dp[n+1];
        dp[0] = 1;
        for(int i=1; i<=n; i++){
            if(s[i-1]=='0')
                if(i>=2 and (s[i-2]=='2' or s[i-2]=='1'))
                    dp[i] = dp[i-2];
                else
                    dp[i] = 0;
            else
                if((i>=2) and ((s[i-2]=='2' and s[i-1]<='6') or (s[i-2]=='1')))
                    dp[i] = dp[i-1] + dp[i-2];
                else
                    dp[i] = dp[i-1];
        } 
        return dp[n];
    }
};
```