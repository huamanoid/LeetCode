# [140. Word Break II (Hard)](https://leetcode.com/problems/word-break-ii/)

<p>Given a string <code>s</code> and a dictionary of strings <code>wordDict</code>, add spaces in <code>s</code> to construct a sentence where each word is a valid dictionary word. Return all such possible sentences in <strong>any order</strong>.</p>

<p><strong>Note</strong> that the same word in the dictionary may be reused multiple times in the segmentation.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> s = "catsanddog", wordDict = ["cat","cats","and","sand","dog"]
<strong>Output:</strong> ["cats and dog","cat sand dog"]
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> s = "pineapplepenapple", wordDict = ["apple","pen","applepen","pine","pineapple"]
<strong>Output:</strong> ["pine apple pen apple","pineapple pen apple","pine applepen apple"]
<strong>Explanation:</strong> Note that you are allowed to reuse a dictionary word.
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> s = "catsandog", wordDict = ["cats","dog","sand","and","cat"]
<strong>Output:</strong> []
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 20</code></li>
	<li><code>1 &lt;= wordDict.length &lt;= 1000</code></li>
	<li><code>1 &lt;= wordDict[i].length &lt;= 10</code></li>
	<li><code>s</code> and <code>wordDict[i]</code> consist of only lowercase English letters.</li>
	<li>All the strings of <code>wordDict</code> are <strong>unique</strong>.</li>
</ul>


**Related Topics**:  
[Hash Table](https://leetcode.com/tag/hash-table/), [String](https://leetcode.com/tag/string/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/), [Backtracking](https://leetcode.com/tag/backtracking/), [Trie](https://leetcode.com/tag/trie/), [Memoization](https://leetcode.com/tag/memoization/)

**Similar Questions**:
* [Word Break (Medium)](https://leetcode.com/problems/word-break/)
* [Concatenated Words (Hard)](https://leetcode.com/problems/concatenated-words/)

## Solution 1. Backtracking

```cpp
// OJ: https://leetcode.com/problems/word-break-ii/
// Author: A M A N
// Time : O(N!)
// Space: O(N)
class Solution {
public:
    vector<string> ans;
    void solve(string s, vector<string>&dict, string prev){
        if(s.size()==0){
            ans.push_back(prev);
            return;
        }
        for(int i=0; i<s.size(); i++){
            string left = s.substr(0, i+1);
            if(find(dict.begin(), dict.end(), left)!=dict.end()){ // if left is present in the dictionary
                string right = s.substr(i+1);
                string prev_new = ""; // "prev" should be preserved for other iterations of the loop, BACKTRACKING
                if(prev=="")  prev_new = left;
                else          prev_new = prev + " " + left;
                solve(right, dict, prev_new);
            }
        }
    }
    vector<string> wordBreak(string s, vector<string>& wordDict) {
        solve(s, wordDict, "");
        return ans;
    }
};
```