# [208. Implement Trie (Prefix Tree) (Medium)](https://leetcode.com/problems/implement-trie-prefix-tree/)

<p>A <a href="https://en.wikipedia.org/wiki/Trie" target="_blank"><strong>trie</strong></a> (pronounced as "try") or <strong>prefix tree</strong> is a tree data structure used to efficiently store and retrieve keys in a dataset of strings. There are various applications of this data structure, such as autocomplete and spellchecker.</p>

<p>Implement the Trie class:</p>

<ul>
	<li><code>Trie()</code> Initializes the trie object.</li>
	<li><code>void insert(String word)</code> Inserts the string <code>word</code> into the trie.</li>
	<li><code>boolean search(String word)</code> Returns <code>true</code> if the string <code>word</code> is in the trie (i.e., was inserted before), and <code>false</code> otherwise.</li>
	<li><code>boolean startsWith(String prefix)</code> Returns <code>true</code> if there is a previously inserted string <code>word</code> that has the prefix <code>prefix</code>, and <code>false</code> otherwise.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input</strong>
["Trie", "insert", "search", "search", "startsWith", "insert", "search"]
[[], ["apple"], ["apple"], ["app"], ["app"], ["app"], ["app"]]
<strong>Output</strong>
[null, null, true, false, true, null, true]

<strong>Explanation</strong>
Trie trie = new Trie();
trie.insert("apple");
trie.search("apple");   // return True
trie.search("app");     // return False
trie.startsWith("app"); // return True
trie.insert("app");
trie.search("app");     // return True
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= word.length, prefix.length &lt;= 2000</code></li>
	<li><code>word</code> and <code>prefix</code> consist only of lowercase English letters.</li>
	<li>At most <code>3 * 10<sup>4</sup></code> calls <strong>in total</strong> will be made to <code>insert</code>, <code>search</code>, and <code>startsWith</code>.</li>
</ul>


**Related Topics**:  
[Hash Table](https://leetcode.com/tag/hash-table/), [String](https://leetcode.com/tag/string/), [Design](https://leetcode.com/tag/design/), [Trie](https://leetcode.com/tag/trie/)

**Similar Questions**:
* [Design Add and Search Words Data Structure (Medium)](https://leetcode.com/problems/design-add-and-search-words-data-structure/)
* [Design Search Autocomplete System (Hard)](https://leetcode.com/problems/design-search-autocomplete-system/)
* [Replace Words (Medium)](https://leetcode.com/problems/replace-words/)
* [Implement Magic Dictionary (Medium)](https://leetcode.com/problems/implement-magic-dictionary/)
* [Implement Trie II (Prefix Tree) (Medium)](https://leetcode.com/problems/implement-trie-ii-prefix-tree/)

## Solution 1. Trie

```cpp
// OJ: https://leetcode.com/problems/implement-trie-prefix-tree/
// Author: A M A N
struct TrieNode {
    bool end;
    TrieNode* children[26] = {nullptr};
    TrieNode(): end(false) {}
};
class Trie {
private:
    TrieNode* root;
public:
    /** Initialize your data structure here. */
    Trie() {
        root = new TrieNode();
    }

    // Time : O(length(word))
    // Space: O(length(word))
    /** Inserts a word into the trie. */
    void insert(string word) {
        auto node = root;
        for(int i=0; i<word.size(); i++){
            int idx = word[i] - 'a';
            if(node->children[idx]==nullptr)
                node->children[idx] = new TrieNode();
            node = node->children[idx];
        }
        node->end = true;
    }
    // Time : O(length(word))
    // Space: O(1)
    /** Returns if the word is in the trie. */
    bool search(string word) {
        auto node = root;
        for(int i=0; i<word.size(); i++){
            int idx = word[i] - 'a';
            if(node->children[idx]==nullptr)
                return false;
            else
                node = node->children[idx];
        }
        return node->end;
    }
    /** Returns if there is any word in the trie that starts with the given prefix. */
    bool startsWith(string word) {
        auto node = root;
        for(int i=0; i<word.size(); i++){
            int idx = word[i] - 'a';
            if(node->children[idx]==nullptr)
                return false;
            else
                node = node->children[idx];
        }
        return true;
    }
};
/**
 * Your Trie object will be instantiated and called as such:
 * Trie* obj = new Trie();
 * obj->insert(word);
 * bool param_2 = obj->search(word);
 * bool param_3 = obj->startsWith(prefix);
 */
```