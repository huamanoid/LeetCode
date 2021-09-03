# [139. Word Break (Medium)](https://leetcode.com/problems/word-break/)

<p>Given a string <code>s</code> and a dictionary of strings <code>wordDict</code>, return <code>true</code> if <code>s</code> can be segmented into a space-separated sequence of one or more dictionary words.</p>

<p><strong>Note</strong> that the same word in the dictionary may be reused multiple times in the segmentation.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> s = "leetcode", wordDict = ["leet","code"]
<strong>Output:</strong> true
<strong>Explanation:</strong> Return true because "leetcode" can be segmented as "leet code".
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> s = "applepenapple", wordDict = ["apple","pen"]
<strong>Output:</strong> true
<strong>Explanation:</strong> Return true because "applepenapple" can be segmented as "apple pen apple".
Note that you are allowed to reuse a dictionary word.
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> s = "catsandog", wordDict = ["cats","dog","sand","and","cat"]
<strong>Output:</strong> false
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 300</code></li>
	<li><code>1 &lt;= wordDict.length &lt;= 1000</code></li>
	<li><code>1 &lt;= wordDict[i].length &lt;= 20</code></li>
	<li><code>s</code> and <code>wordDict[i]</code> consist of only lowercase English letters.</li>
	<li>All the strings of <code>wordDict</code> are <strong>unique</strong>.</li>
</ul>


**Related Topics**:  
[Hash Table](https://leetcode.com/tag/hash-table/), [String](https://leetcode.com/tag/string/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/), [Trie](https://leetcode.com/tag/trie/), [Memoization](https://leetcode.com/tag/memoization/)

**Similar Questions**:
* [Word Break II (Hard)](https://leetcode.com/problems/word-break-ii/)

## Solution 1. DP (Recursive)

```cpp
// OJ: https://leetcode.com/problems/word-break/
// Author: A M A N
// Time : O(N^3)
// Space: O(N^2)
class Solution {
public:
    unordered_set<string> sett;
    unordered_map<string, bool> dp;
    bool solve(string s, int start, int end){
        string ss = s.substr(start, end-start+1);
        if(sett.count(ss))
            return true;
        if(dp.find(ss)!=dp.end())
            return dp[ss];
        for(int k = start; k < end; k++){
            bool left, right;
            if(dp.find(s.substr(start, k-start+1))!=dp.end())
                   left = dp[s.substr(start, k-start+1)];
            else   left  = solve(s, start, k);
            if(dp.find(s.substr(k+1, end-(k+1)+1))!=dp.end())
                    right = dp[s.substr(k+1, end-(k+1)+1)];
            else    right = solve(s, k+1, end);
            if(left and right)
                return dp[ss] = true;
        }
        return dp[ss] = false;
    }
    bool wordBreak(string s, vector<string>& wordDict) {
        for(auto s: wordDict)
            sett.insert(s);
        return solve(s, 0, s.size()-1);
    }
};
```