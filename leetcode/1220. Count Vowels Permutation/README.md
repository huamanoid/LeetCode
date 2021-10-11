# [1220. Count Vowels Permutation (Hard)](https://leetcode.com/problems/count-vowels-permutation/)

<p>Given an integer <code>n</code>, your task is to count how many strings of length <code>n</code> can be formed under the following rules:</p>

<ul>
	<li>Each character is a lower case vowel&nbsp;(<code>'a'</code>, <code>'e'</code>, <code>'i'</code>, <code>'o'</code>, <code>'u'</code>)</li>
	<li>Each vowel&nbsp;<code>'a'</code> may only be followed by an <code>'e'</code>.</li>
	<li>Each vowel&nbsp;<code>'e'</code> may only be followed by an <code>'a'</code>&nbsp;or an <code>'i'</code>.</li>
	<li>Each vowel&nbsp;<code>'i'</code> <strong>may not</strong> be followed by another <code>'i'</code>.</li>
	<li>Each vowel&nbsp;<code>'o'</code> may only be followed by an <code>'i'</code> or a&nbsp;<code>'u'</code>.</li>
	<li>Each vowel&nbsp;<code>'u'</code> may only be followed by an <code>'a'.</code></li>
</ul>

<p>Since the answer&nbsp;may be too large,&nbsp;return it modulo <code>10^9 + 7.</code></p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> n = 1
<strong>Output:</strong> 5
<strong>Explanation:</strong> All possible strings are: "a", "e", "i" , "o" and "u".
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> n = 2
<strong>Output:</strong> 10
<strong>Explanation:</strong> All possible strings are: "ae", "ea", "ei", "ia", "ie", "io", "iu", "oi", "ou" and "ua".
</pre>

<p><strong>Example 3:&nbsp;</strong></p>

<pre><strong>Input:</strong> n = 5
<strong>Output:</strong> 68</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 2 * 10^4</code></li>
</ul>


**Companies**:  
[Atlassian](https://leetcode.com/company/atlassian)

**Related Topics**:  
[Dynamic Programming](https://leetcode.com/tag/dynamic-programming/)

## Solution 1. Recursion + Memoization

```cpp
// OJ: https://leetcode.com/problems/count-vowels-permutation/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    static const int M = 1e9 + 7;
    map<char, vector<char>> mp;
    unordered_map<int, unordered_map<char, int>> dp;

    int solve(int n, char c){
        if(n==0) return 1;
        if(dp.find(n)!=dp.end() and dp[n].find(c)!=dp[n].end())
            return dp[n][c];
        int ans = 0;
        for(auto next: mp[c])
            (ans += solve(n-1, next))%=M;;
        return dp[n][c] = ans;
    }

    int countVowelPermutation(int n) {
        mp['a'] = {'e'};
        mp['e'] = {'a', 'i'};
        mp['i'] = {'a', 'e', 'o', 'u'};
        mp['o'] = {'i', 'u'};
        mp['u'] = {'a'};
        int ans = 0;
        for(char c: {'a', 'e', 'i', 'o', 'u'})
            (ans += solve(n-1, c))%=M;
        return ans;
    }
};
```