# [336. Palindrome Pairs (Hard)](https://leetcode.com/problems/palindrome-pairs/)

<p>Given a list of <b>unique</b> words, return all the pairs of the&nbsp;<b><i>distinct</i></b> indices <code>(i, j)</code> in the given list, so that the concatenation of the two words&nbsp;<code>words[i] + words[j]</code> is a palindrome.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> words = ["abcd","dcba","lls","s","sssll"]
<strong>Output:</strong> [[0,1],[1,0],[3,2],[2,4]]
<strong>Explanation:</strong> The palindromes are ["dcbaabcd","abcddcba","slls","llssssll"]
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> words = ["bat","tab","cat"]
<strong>Output:</strong> [[0,1],[1,0]]
<strong>Explanation:</strong> The palindromes are ["battab","tabbat"]
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> words = ["a",""]
<strong>Output:</strong> [[0,1],[1,0]]
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 5000</code></li>
	<li><code>0 &lt;= words[i].length &lt;= 300</code></li>
	<li><code>words[i]</code> consists of lower-case English letters.</li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Hash Table](https://leetcode.com/tag/hash-table/), [String](https://leetcode.com/tag/string/), [Trie](https://leetcode.com/tag/trie/)

**Similar Questions**:
* [Longest Palindromic Substring (Medium)](https://leetcode.com/problems/longest-palindromic-substring/)
* [Shortest Palindrome (Hard)](https://leetcode.com/problems/shortest-palindrome/)

## Solution 1. Trie

```cpp
// OJ: https://leetcode.com/problems/palindrome-pairs/
// Author: A M A N
// Time : O(N*M*M)
// Space: O(N)
 class Trie{
private:
    struct TrieNode{
        bool end = false;
        TrieNode* children[26] = {nullptr};
    };
    TrieNode* root;
public:
    Trie(){
        root = new TrieNode();
    }
    
    void insert(string &word){
        auto node = root;
        for(char &c: word){
            int idx = c-'a';
            if(node->children[idx]==nullptr)
                node->children[idx] = new TrieNode();
            node = node->children[idx];
        }
        node->end = true;
    }
    
    
    bool search(string &word){
        auto node = root;
        for(char &c: word){
            int idx = c-'a';
            if(node->children[idx]==nullptr)
                return false;
            node = node->children[idx];
        } 
        return node->end;
    }
    
};

bool isPalindrome(string& s){
    int i=0, j=s.size()-1;
    while(i<j)
        if(s[i++]!=s[j--])
            return false;
    return true;
}

class Solution {
public:
    
    vector<vector<int>> palindromePairs(vector<string>& words) {
        Trie* t = new Trie();
        
        unordered_map<string, int> idx;
        for(int i=0; i<words.size(); i++){
            string s = words[i];
            reverse(s.begin(), s.end());
            idx[s] = i;
            t->insert(s);

        }
    
        vector<vector<int>> ans;
        if(idx.count("")) // handling special case seperately
            for(int i=0; i<words.size(); i++)
                if(words[i]!="" and isPalindrome(words[i]))
                    ans.push_back({idx[""], i});

        for(int i=0; i<words.size(); i++){
            for(int j=0; j<words[i].size(); j++){
                string left  = words[i].substr(0, j); // break into left and righ substrings
                string right = words[i].substr(j);
                if(t->search(left) and isPalindrome(right)) 
                    if(idx[left]!=i)
                        ans.push_back({i, idx[left]}); //  left + right + foundedWord (i.e. rev(left))
                if(t->search(right) and isPalindrome(left))
                    if(idx[right]!=i)
                        ans.push_back({idx[right], i}); // foundedWord(i.e. rev(right)) + left + right 
            }            
        }
        return ans;
    }
};
```