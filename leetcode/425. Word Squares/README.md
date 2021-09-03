# [425. Word Squares (Hard)](https://leetcode.com/problems/word-squares/)

<p>Given a set of words without duplicates, find all word squares you can build from them.</p>

<p>A sequence of words forms a valid word square if the kth row and column read the exact same string, where 0 ≤ k < max(numRows, numColumns).</p>

<p>For example, the word sequence ["ball","area","lead","lady"] forms a word square because each word <code>reads the same both horizontally and vertically</code>.</p>

```
b a l l
a r e a
l e a d
l a d y
```


<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input</strong>
["area","lead","wall","lady","ball"]

<strong>Output</strong>
[["wall","area","lead","lady"],["ball","area","lead","lady"]]

<strong>Explanation</strong>
The output consists of two word squares. The order of output does not matter (just the order of words in each word square matters).
</pre>


<p>&nbsp;</p>
<p><strong>Example 2:</strong></p>

<pre><strong>Input</strong>
["abat","baba","atan","atal"]

<strong>Output</strong>
 [["baba","abat","baba","atan"],["baba","abat","baba","atal"]]

</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>There are at least 1 and at most 1000 words.</code></li>
	<li><code>All words will have the exact same length.</code></li>
	<li><code>Word length is at least 1 and at most 5.</code></li>
	<li><code>Each word contains only lowercase English alphabet a-z.</code></li>
</ul>


**Related Topics**:  
[Trie](https://leetcode.com/tag/trie/)


## Solution 1. Trie + Backtracking


```cpp
// OJ: https://www.lintcode.com/problem/634/
// Author: A M A N
// Time : 
// Space: 
class Trie{
    struct TrieNode{
        bool end = false;
        TrieNode* children[26] = {nullptr};
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
    
    bool wordsWithPrefix(string word, vector<string>& ans){
        auto node = root;
        for(char c: word){
            int idx = c-'a';
            if(node->children[idx]==nullptr)
                return false;
            node = node->children[idx];
        }
        dfs(node, word, ans); // find all the words with word as a prefix
        return true;
    }
    
    void dfs(TrieNode* node, string temp, vector<string>& wordsWithPrefix){
        if(node->end){
            wordsWithPrefix.push_back(temp);
            return;
        }
        for(int i=0; i<26; i++)
            if(node->children[i])
                dfs(node->children[i], temp+string(1, i+'a'), wordsWithPrefix);
    }

    void wordSquare(vector<string>& words, int n, vector<string> temp, vector<vector<string>>&ans){
        if(temp.size()==n){
            ans.push_back(temp);
            return;
        }
        
        int m = temp.size();
        string prefix;
        for(int i=0; i<temp.size(); i++){
                prefix+=temp[i][m];
        }
        
        vector<string> candidateWords;
        if(wordsWithPrefix(prefix, candidateWords)){
            for(string s: candidateWords){
                temp.push_back(s);
                wordSquare(words, n, temp, ans);
                temp.pop_back(); //backtrack
            }
        }

    }
};


class Solution {
public:

    vector<vector<string>> wordSquares(vector<string> &words) {
        vector<vector<string>> ans;
        if(words.size()==0)return ans;
        int n = words[0].size();
        Trie* t = new Trie();
        for(auto s: words)
            t->insert(s);
        t->wordSquare(words, n, vector<string>(), ans);
        return ans; 
    }
};
```