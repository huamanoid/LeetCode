# [115. Distinct Subsequences (Hard)](https://leetcode.com/problems/distinct-subsequences/)

<p>Given two strings <code>s</code> and <code>t</code>, return <em>the number of distinct subsequences of <code>s</code> which equals <code>t</code></em>.</p>

<p>A string's <strong>subsequence</strong> is a new string formed from the original string by deleting some (can be none) of the characters without disturbing the remaining characters' relative positions. (i.e., <code>"ACE"</code> is a subsequence of <code>"ABCDE"</code> while <code>"AEC"</code> is not).</p>

<p>It is guaranteed the answer fits on a 32-bit signed integer.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> s = "rabbbit", t = "rabbit"
<strong>Output:</strong> 3
<strong>Explanation:</strong>
As shown below, there are 3 ways you can generate "rabbit" from S.
<code><strong><u>rabb</u></strong>b<strong><u>it</u></strong></code>
<code><strong><u>ra</u></strong>b<strong><u>bbit</u></strong></code>
<code><strong><u>rab</u></strong>b<strong><u>bit</u></strong></code>
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> s = "babgbag", t = "bag"
<strong>Output:</strong> 5
<strong>Explanation:</strong>
As shown below, there are 5 ways you can generate "bag" from S.
<code><strong><u>ba</u></strong>b<u><strong>g</strong></u>bag</code>
<code><strong><u>ba</u></strong>bgba<strong><u>g</u></strong></code>
<code><u><strong>b</strong></u>abgb<strong><u>ag</u></strong></code>
<code>ba<u><strong>b</strong></u>gb<u><strong>ag</strong></u></code>
<code>babg<strong><u>bag</u></strong></code></pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= s.length, t.length &lt;= 1000</code></li>
	<li><code>s</code> and <code>t</code> consist of English letters.</li>
</ul>


**Companies**:  
[Amazon](https://leetcode.com/company/amazon), [Mathworks](https://leetcode.com/company/mathworks), [Google](https://leetcode.com/company/google)

**Related Topics**:  
[String](https://leetcode.com/tag/string/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/)

**Similar Questions**:
* [Number of Unique Good Subsequences (Hard)](https://leetcode.com/problems/number-of-unique-good-subsequences/)

## Solution 1. Recursion + Memoization

```cpp
// OJ: https://leetcode.com/problems/distinct-subsequences/
// Author: A M A N
// Time : O()
// Space: O()
class Solution {
public:
    string end;
    unordered_map<int, unordered_map<string, int>>dp;
    int solve(string& s, int idx, string& temp){
        if(temp==end)return 1;
        if(idx>=s.size()) return 0;
        if(dp.find(idx)!=dp.end() and dp[idx].find(temp)!=dp[idx].end())
            return dp[idx][temp];
        int ans = 0;
        if(s[idx] == end[temp.size()]){
            temp+=s[idx];
            ans += solve(s, idx+1, temp);// pick if valid
            temp.pop_back();
        }
        ans += solve(s, idx+1, temp); //skip
        return dp[idx][temp] = ans;
    }
    int numDistinct(string s, string t) {
        this->end = t;
        string temp;
        return solve(s, 0, temp);
    }
};
```