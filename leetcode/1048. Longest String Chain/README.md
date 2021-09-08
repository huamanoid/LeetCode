# [1048. Longest String Chain (Medium)](https://leetcode.com/problems/longest-string-chain/)

<p>You are given an array of <code>words</code> where each word consists of lowercase English letters.</p>

<p><code>word<sub>A</sub></code> is a <strong>predecessor</strong> of <code>word<sub>B</sub></code> if and only if we can insert <strong>exactly one</strong> letter anywhere in <code>word<sub>A</sub></code> <strong>without changing the order of the other characters</strong> to make it equal to <code>word<sub>B</sub></code>.</p>

<ul>
	<li>For example, <code>"abc"</code> is a <strong>predecessor</strong> of <code>"ab<u>a</u>c"</code>, while <code>"cba"</code> is not a <strong>predecessor</strong> of <code>"bcad"</code>.</li>
</ul>

<p>A <strong>word chain</strong><em> </em>is a sequence of words <code>[word<sub>1</sub>, word<sub>2</sub>, ..., word<sub>k</sub>]</code> with <code>k &gt;= 1</code>, where <code>word<sub>1</sub></code> is a <strong>predecessor</strong> of <code>word<sub>2</sub></code>, <code>word<sub>2</sub></code> is a <strong>predecessor</strong> of <code>word<sub>3</sub></code>, and so on. A single word is trivially a <strong>word chain</strong> with <code>k == 1</code>.</p>

<p>Return <em>the <strong>length</strong> of the <strong>longest possible word chain</strong> with words chosen from the given list of </em><code>words</code>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> words = ["a","b","ba","bca","bda","bdca"]
<strong>Output:</strong> 4
<strong>Explanation</strong>: One of the longest word chains is ["a","<u>b</u>a","b<u>d</u>a","bd<u>c</u>a"].
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> words = ["xbc","pcxbcf","xb","cxbc","pcxbc"]
<strong>Output:</strong> 5
<strong>Explanation:</strong> All the words can be put in a word chain ["xb", "xb<u>c</u>", "<u>c</u>xbc", "<u>p</u>cxbc", "pcxbc<u>f</u>"].
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> words = ["abcd","dbqca"]
<strong>Output:</strong> 1
<strong>Explanation:</strong> The trivial word chain ["abcd"] is one of the longest word chains.
["abcd","dbqca"] is not a valid word chain because the ordering of the letters is changed.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 1000</code></li>
	<li><code>1 &lt;= words[i].length &lt;= 16</code></li>
	<li><code>words[i]</code> only consists of lowercase English letters.</li>
</ul>


**Companies**:  
[Google](https://leetcode.com/company/google), [Two Sigma](https://leetcode.com/company/two-sigma), [Mathworks](https://leetcode.com/company/mathworks), [Microsoft](https://leetcode.com/company/microsoft)

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Hash Table](https://leetcode.com/tag/hash-table/), [Two Pointers](https://leetcode.com/tag/two-pointers/), [String](https://leetcode.com/tag/string/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/)

## Solution 1. Recursion + Memoization

```cpp
// OJ: https://leetcode.com/problems/longest-string-chain/
// Author: A M A N
// Time : O((N^2)*(L^2))
// Space: O(N)
class Solution {
public:
    bool isPredecessor(string u, string pu){
        if(pu.size()!=u.size()+1)return false;
        int i=0, j=0, skip=0;
        while(i<u.size() and j<pu.size())
            if(u[i]==pu[j])i++, j++;
            else j++, skip++;
        return skip<=1;
    }
    unordered_map<string , int> dp;
    int solve(string u, unordered_map<string, vector<string>>& p){
        if(p.find(u)==p.end()) return 1;
        if(dp.find(u)!=dp.end())
            return dp[u];
        int ans = 1;
        for(auto pu: p[u])
            ans = max(ans, 1+solve(pu, p));
        return dp[u] = ans;
    }
    int longestStrChain(vector<string>& words) {
        unordered_map<int, vector<string>> wordsWithSize; // maintain words by length
        int maxLength=0;
        for(auto &w: words){
            int n = w.size();
            wordsWithSize[n].push_back(w);
            maxLength = max(maxLength, n);
        }
        unordered_map<string, vector<string>> p; // predecessor map
        for(int L = 1; L < maxLength; L++){
            for(auto u: wordsWithSize[L])
                for(auto pu: wordsWithSize[L+1])
                    if(isPredecessor(u, pu)){
                        p[u].push_back(pu);
                    }
        }
        int longestChain = 1;
        for(auto s: words)
            longestChain = max(longestChain, solve(s, p));
        return longestChain;
    }
};
```

A little different approach.

## Solution 2. Recursion + Memoization

```cpp
// OJ: https://leetcode.com/problems/longest-string-chain/solution/
// Author: A M A N
// Time : O(N*L^2)
// Space: O(N)
class Solution {
public:
    unordered_set<string> sett;
    unordered_map<string, int> dp;
    int solve(vector<string>&words, string s){             //O(N)
        if(dp.find(s)!=dp.end())
            return dp[s];
        int ans = 1;
        for(int i=0; i<s.size(); i++){                      //O(L)
            string child = s.substr(0, i) + s.substr(i+1);  //O(L)
            if(sett.count(child))
                ans = max(ans, 1 + solve(words, child));
        }
        return dp[s] = ans;
    }
    int longestStrChain(vector<string>& words) {
        for(string s: words)sett.insert(s);
        int res = 1;
        for(auto w: words)
            res = max(res, solve(words, w));
        return res;
    }   
};
```