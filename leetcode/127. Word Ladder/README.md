# [127. Word Ladder (Hard)](https://leetcode.com/problems/word-ladder/)

<p>A <strong>transformation sequence</strong> from word <code>beginWord</code> to word <code>endWord</code> using a dictionary <code>wordList</code> is a sequence of words <code>beginWord -&gt; s<sub>1</sub> -&gt; s<sub>2</sub> -&gt; ... -&gt; s<sub>k</sub></code> such that:</p>

<ul>
	<li>Every adjacent pair of words differs by a single letter.</li>
	<li>Every <code>s<sub>i</sub></code> for <code>1 &lt;= i &lt;= k</code> is in <code>wordList</code>. Note that <code>beginWord</code> does not need to be in <code>wordList</code>.</li>
	<li><code>s<sub>k</sub> == endWord</code></li>
</ul>

<p>Given two words, <code>beginWord</code> and <code>endWord</code>, and a dictionary <code>wordList</code>, return <em>the <strong>number of words</strong> in the <strong>shortest transformation sequence</strong> from</em> <code>beginWord</code> <em>to</em> <code>endWord</code><em>, or </em><code>0</code><em> if no such sequence exists.</em></p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]
<strong>Output:</strong> 5
<strong>Explanation:</strong> One shortest transformation sequence is "hit" -&gt; "hot" -&gt; "dot" -&gt; "dog" -&gt; cog", which is 5 words long.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log"]
<strong>Output:</strong> 0
<strong>Explanation:</strong> The endWord "cog" is not in wordList, therefore there is no valid transformation sequence.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= beginWord.length &lt;= 10</code></li>
	<li><code>endWord.length == beginWord.length</code></li>
	<li><code>1 &lt;= wordList.length &lt;= 5000</code></li>
	<li><code>wordList[i].length == beginWord.length</code></li>
	<li><code>beginWord</code>, <code>endWord</code>, and <code>wordList[i]</code> consist of lowercase English letters.</li>
	<li><code>beginWord != endWord</code></li>
	<li>All the words in <code>wordList</code> are <strong>unique</strong>.</li>
</ul>


**Related Topics**:  
[Hash Table](https://leetcode.com/tag/hash-table/), [String](https://leetcode.com/tag/string/), [Breadth-First Search](https://leetcode.com/tag/breadth-first-search/)

**Similar Questions**:
* [Word Ladder II (Hard)](https://leetcode.com/problems/word-ladder-ii/)
* [Minimum Genetic Mutation (Medium)](https://leetcode.com/problems/minimum-genetic-mutation/)

## Solution 1. BFS

Since we're looking for the shortest path, BFS should be our first option

```cpp
// OJ: https://leetcode.com/problems/word-ladder/
// Author: A M A N
// Time : O(N * W^2)
// Space: O(N * W)
class Solution {
public:
    vector<string> similarWord(string& s, unordered_set<string>&sett){
        vector<string> res;
        for(int i=0; i<s.size(); i++){ 
            string temp = s;
            for(int j=0; j<26; j++){
                temp[i] = j+'a';
                if(temp!=s and sett.count(temp))
                    res.push_back(temp);
            }
        }
        return res;
    }
    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
        unordered_set<string> sett(wordList.begin(), wordList.end());  
        if(!sett.count(endWord))
            return 0;
        int ans = 0;
        queue<string> q;
        q.push(beginWord);
        unordered_set<string>vis;
        vis.insert(beginWord);
        while(!q.empty()){
            for(int i=q.size(); i>0; i--){
                string s = q.front(); q.pop();
                vector<string> nexts = similarWord(s, sett);
                for(auto e: nexts){
                    if(e==endWord)
                        return ans+2;
                    else if(!vis.count(e)){ // we can also erase the used string from sett instead of using visited array
                        vis.insert(e);
                        q.push(e);
                    }
                }
            }
            ans++;
        }
        return 0;
    }
};
```