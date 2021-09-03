# [126. Word Ladder II (Hard)](https://leetcode.com/problems/word-ladder-ii/)

<p>A <strong>transformation sequence</strong> from word <code>beginWord</code> to word <code>endWord</code> using a dictionary <code>wordList</code> is a sequence of words <code>beginWord -&gt; s<sub>1</sub> -&gt; s<sub>2</sub> -&gt; ... -&gt; s<sub>k</sub></code> such that:</p>

<ul>
	<li>Every adjacent pair of words differs by a single letter.</li>
	<li>Every <code>s<sub>i</sub></code> for <code>1 &lt;= i &lt;= k</code> is in <code>wordList</code>. Note that <code>beginWord</code> does not need to be in <code>wordList</code>.</li>
	<li><code>s<sub>k</sub> == endWord</code></li>
</ul>

<p>Given two words, <code>beginWord</code> and <code>endWord</code>, and a dictionary <code>wordList</code>, return <em>all the <strong>shortest transformation sequences</strong> from</em> <code>beginWord</code> <em>to</em> <code>endWord</code><em>, or an empty list if no such sequence exists. Each sequence should be returned as a list of the words </em><code>[beginWord, s<sub>1</sub>, s<sub>2</sub>, ..., s<sub>k</sub>]</code>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]
<strong>Output:</strong> [["hit","hot","dot","dog","cog"],["hit","hot","lot","log","cog"]]
<strong>Explanation:</strong>&nbsp;There are 2 shortest transformation sequences:
"hit" -&gt; "hot" -&gt; "dot" -&gt; "dog" -&gt; "cog"
"hit" -&gt; "hot" -&gt; "lot" -&gt; "log" -&gt; "cog"
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log"]
<strong>Output:</strong> []
<strong>Explanation:</strong> The endWord "cog" is not in wordList, therefore there is no valid transformation sequence.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= beginWord.length &lt;= 5</code></li>
	<li><code>endWord.length == beginWord.length</code></li>
	<li><code>1 &lt;= wordList.length &lt;= 1000</code></li>
	<li><code>wordList[i].length == beginWord.length</code></li>
	<li><code>beginWord</code>, <code>endWord</code>, and <code>wordList[i]</code> consist of lowercase English letters.</li>
	<li><code>beginWord != endWord</code></li>
	<li>All the words in <code>wordList</code> are <strong>unique</strong>.</li>
</ul>


**Related Topics**:  
[Hash Table](https://leetcode.com/tag/hash-table/), [String](https://leetcode.com/tag/string/), [Backtracking](https://leetcode.com/tag/backtracking/), [Breadth-First Search](https://leetcode.com/tag/breadth-first-search/)

**Similar Questions**:
* [Word Ladder (Hard)](https://leetcode.com/problems/word-ladder/)

## Solution 1. BFS of Paths

It can be solved with standard BFS. The tricky idea is doing BFS of paths instead of words! Then the queue becomes a queue of paths.

```cpp
// OJ: https://leetcode.com/problems/word-ladder-ii/
// Author: A M A N
// Time : O()
// Space: O()
class Solution {
class Solution {
public:
    vector<vector<string>> findLadders(string beginWord, string endWord, vector<string>& wordList) {
        unordered_set<string> sett(wordList.begin(), wordList.end());
        unordered_set<string> vis;
        vector<vector<string>> ans;
        queue<vector<string>> q;
        q.push({beginWord});
        while(!q.empty()){
            for(int i = q.size(); i>0; i--){
                vector<string> cur_path = q.front(); q.pop();
                string last = cur_path.back();
                for(int j=0; j<last.size(); j++){
                    string temp = last;
                    for(int k=0; k<26; k++){
                        temp[j] = k+'a';
                        if(temp!=last and sett.count(temp)){
                            //path will be reused in the loop
                            //so copy a new path
                            vector<string> newPath = cur_path;
                            newPath.push_back(temp);
                            vis.insert(temp);
                            if(temp==endWord)
                                ans.push_back(newPath);
                            else
                                q.push(newPath);
                        }
                    }
                }
            }
        //"visited" records all the visited nodes on this level
        //these words will never be visited again after this level 
        //and should be removed from wordList. This is guaranteed
        // by the shortest path.
            for(auto s: vis)
                sett.erase(s);
        }
        return ans;
    }
};
```


## Solution 2. Backtracking (TLE)

```cpp
class Solution {
public:
    
    vector<vector<string>> ans;
    unordered_set<string> sett;
    
    
    void solve(string start, string end, unordered_set<string> vis, vector<string>temp){
        if(start==end){
            ans.push_back(temp);
            return;
        }
        
        for(int i=0; i<start.size(); i++){
            string next = start;
            for(int j=0; j<26; j++){
                next[i] = j + 'a';
                if(sett.count(next) and next!=start and !vis.count(next)){
                    temp.push_back(next);
                    vis.insert(next);
                    solve(next, end, vis, temp);
                    vis.erase(next);
                    temp.pop_back();
                }
            }
        }
    }
    
    vector<vector<string>> findLadders(string beginWord, string endWord, vector<string>& wordList) {
        vector<string> temp = {beginWord};
        for(auto e:wordList) sett.insert(e);
        unordered_set<string> vis;
        solve(beginWord, endWord, vis, temp);
        vector<vector<string>>res;
        int mx = 1e9;
        for(auto a:ans)  mx = min(mx, (int)a.size());
        for(auto a: ans)
            if(a.size()==mx)
                res.push_back(a);
        return res;
    }
};
```