# [211. Design Add and Search Words Data Structure (Medium)](https://leetcode.com/problems/design-add-and-search-words-data-structure/)

<p>Design a data structure that supports adding new words and finding if a string matches any previously added string.</p>

<p>Implement the <code>WordDictionary</code> class:</p>

<ul>
	<li><code>WordDictionary()</code>&nbsp;Initializes the object.</li>
	<li><code>void addWord(word)</code> Adds <code>word</code> to the data structure, it can be matched later.</li>
	<li><code>bool search(word)</code>&nbsp;Returns <code>true</code> if there is any string in the data structure that matches <code>word</code>&nbsp;or <code>false</code> otherwise. <code>word</code> may contain dots <code>'.'</code> where dots can be matched with any letter.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Example:</strong></p>

<pre><strong>Input</strong>
["WordDictionary","addWord","addWord","addWord","search","search","search","search"]
[[],["bad"],["dad"],["mad"],["pad"],["bad"],[".ad"],["b.."]]
<strong>Output</strong>
[null,null,null,null,false,true,true,true]

<strong>Explanation</strong>
WordDictionary wordDictionary = new WordDictionary();
wordDictionary.addWord("bad");
wordDictionary.addWord("dad");
wordDictionary.addWord("mad");
wordDictionary.search("pad"); // return False
wordDictionary.search("bad"); // return True
wordDictionary.search(".ad"); // return True
wordDictionary.search("b.."); // return True
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= word.length &lt;= 500</code></li>
	<li><code>word</code> in <code>addWord</code> consists lower-case English letters.</li>
	<li><code>word</code> in <code>search</code> consist of&nbsp; <code>'.'</code> or lower-case English letters.</li>
	<li>At most <code>50000</code>&nbsp;calls will be made to <code>addWord</code>&nbsp;and <code>search</code>.</li>
</ul>


**Related Topics**:  
[String](https://leetcode.com/tag/string/), [Depth-First Search](https://leetcode.com/tag/depth-first-search/), [Design](https://leetcode.com/tag/design/), [Trie](https://leetcode.com/tag/trie/)

**Similar Questions**:
* [Implement Trie (Prefix Tree) (Medium)](https://leetcode.com/problems/implement-trie-prefix-tree/)
* [Prefix and Suffix Search (Hard)](https://leetcode.com/problems/prefix-and-suffix-search/)

## Solution 1. Trie

```cpp
// OJ: https://leetcode.com/problems/design-add-and-search-words-data-structure/
// Author: A M A N
// Time : O()
// Space: O()
class WordDictionary {
public:
    struct TrieNode{
        bool end;
        TrieNode* children[26] = {nullptr};
        TrieNode(){
            end = false;
        }
    };
    TrieNode* root;
    /** Initialize your data structure here. */
    WordDictionary() {
        root = new TrieNode();
    }
    void addWord(string word) {
        auto node = root;
        for(char c: word){
            int idx = c - 'a';
            if(node->children[idx]==nullptr)
                node->children[idx] = new TrieNode();
            node = node->children[idx];
        }
        node->end = true;
    }
    bool search(string word) {
        return searchUtil(word, 0, root);
    }
    bool searchUtil(string &word, int start, TrieNode* node){
        for(int i=start; i<word.size(); i++){
            int idx = word[i] - 'a';
            if(word[i]=='.'){
                for(int j=0; j<26; j++)
                    if(node->children[j])
                        if(searchUtil(word, i+1, node->children[j]))
                            return true;
                return false;
            }else{
                if(node->children[idx]==nullptr)
                    return false;
                node = node->children[idx];
            }
        }
        return node->end;
    }
};
/**
 * Your WordDictionary object will be instantiated and called as such:
 * WordDictionary* obj = new WordDictionary();
 * obj->addWord(word);
 * bool param_2 = obj->search(word);
 */
```