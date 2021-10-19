# [720. Longest Word in Dictionary (Medium)](https://leetcode.com/problems/longest-word-in-dictionary/)

<p>Given an array of strings <code>words</code> representing an English Dictionary, return <em>the longest word in</em> <code>words</code> <em>that can be built one character at a time by other words in</em> <code>words</code>.</p>

<p>If there is more than one possible answer, return the longest word with the smallest lexicographical order. If there is no answer, return the empty string.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> words = ["w","wo","wor","worl","world"]
<strong>Output:</strong> "world"
<strong>Explanation:</strong> The word "world" can be built one character at a time by "w", "wo", "wor", and "worl".
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> words = ["a","banana","app","appl","ap","apply","apple"]
<strong>Output:</strong> "apple"
<strong>Explanation:</strong> Both "apply" and "apple" can be built from other words in the dictionary. However, "apple" is lexicographically smaller than "apply".
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 1000</code></li>
	<li><code>1 &lt;= words[i].length &lt;= 30</code></li>
	<li><code>words[i]</code> consists of lowercase English letters.</li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Hash Table](https://leetcode.com/tag/hash-table/), [String](https://leetcode.com/tag/string/), [Trie](https://leetcode.com/tag/trie/), [Sorting](https://leetcode.com/tag/sorting/)

**Similar Questions**:
* [Longest Word in Dictionary through Deleting (Medium)](https://leetcode.com/problems/longest-word-in-dictionary-through-deleting/)
* [Implement Magic Dictionary (Medium)](https://leetcode.com/problems/implement-magic-dictionary/)
* [Longest Word With All Prefixes (Medium)](https://leetcode.com/problems/longest-word-with-all-prefixes/)

## Solution 1. Trie

```cpp
// OJ: https://leetcode.com/problems/longest-word-in-dictionary/
// Author: A M A N
// Time : O(Total(chars))
// Space: O(Total(chars))
class Trie{
private:
    struct TrieNode{
        bool end;
        TrieNode* children[26] = {nullptr};
        TrieNode(){
            end = false;
        }
    };
    TrieNode* root;
public:
    Trie(){
        root = new TrieNode();
    }
    void insert(string word){
        auto node = root;
        for(char c: word){
            int idx = c-'a';
            if(node->children[idx]==nullptr)
                node->children[idx] = new TrieNode();
            node = node->children[idx];
        }
        node->end = true;
    }
    void dfs(TrieNode* node, string temp, string& longest){
        for(int i=0; i<26; i++)
            if(node->children[i] and node->children[i]->end)
                dfs(node->children[i], temp+string(1, i+'a'), longest);
        // maintaing longest string
        if(temp.size() > longest.size()){
            longest = temp;
        }else if(temp.size()==longest.size()){
            if(temp<longest) // longest should be lexicographically shorter
                longest = temp;
        }
    }
    string longestWord(){
        string longest="";
        dfs(root, "", longest);
        return longest;
    }
};
class Solution {
public:
    string longestWord(vector<string>& words) {
        Trie* t = new Trie();
        for(string s: words)
            t->insert(s);
        return t->longestWord();
    }
};
```

## Solution 2. Hashing

```cpp
// OJ: https://leetcode.com/problems/longest-word-in-dictionary/
// Author: A M A N
// Time : O()
// Space: O()
class Solution {
public:
    unordered_set<string> dict;
    string solve(string s){
        string ans = s;
        for(char c='a'; c<='z'; c++){
            string ss = s + c;
            if(dict.count(ss)){
                string temp = solve(ss);
                if(temp.size()>ans.size())
                    ans = temp;
                else if(temp.size()==ans.size())
                    if(temp < ans)
                        ans = temp;
            }
        }
        return ans;
    }
    string longestWord(vector<string>& words) {
        for(auto &s : words)
            dict.insert(s);
        return solve("");
    }
};
```