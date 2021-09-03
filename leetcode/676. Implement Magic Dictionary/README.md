# [676. Implement Magic Dictionary (Medium)](https://leetcode.com/problems/implement-magic-dictionary/)

<p>Design a data structure that is initialized with a list of <strong>different</strong> words. Provided a string, you should determine if you can change exactly one character in this string to match any word in the data structure.</p>

<p>Implement the&nbsp;<code>MagicDictionary</code>&nbsp;class:</p>

<ul>
	<li><code>MagicDictionary()</code>&nbsp;Initializes the object.</li>
	<li><code>void buildDict(String[]&nbsp;dictionary)</code>&nbsp;Sets the data structure&nbsp;with an array of distinct strings <code>dictionary</code>.</li>
	<li><code>bool search(String searchWord)</code> Returns <code>true</code> if you can change <strong>exactly one character</strong> in <code>searchWord</code> to match any string in the data structure, otherwise returns <code>false</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input</strong>
["MagicDictionary", "buildDict", "search", "search", "search", "search"]
[[], [["hello", "leetcode"]], ["hello"], ["hhllo"], ["hell"], ["leetcoded"]]
<strong>Output</strong>
[null, null, false, true, false, false]

<strong>Explanation</strong>
MagicDictionary magicDictionary = new MagicDictionary();
magicDictionary.buildDict(["hello", "leetcode"]);
magicDictionary.search("hello"); // return False
magicDictionary.search("hhllo"); // We can change the second 'h' to 'e' to match "hello" so we return True
magicDictionary.search("hell"); // return False
magicDictionary.search("leetcoded"); // return False
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;=&nbsp;dictionary.length &lt;= 100</code></li>
	<li><code>1 &lt;=&nbsp;dictionary[i].length &lt;= 100</code></li>
	<li><code>dictionary[i]</code> consists of only lower-case English letters.</li>
	<li>All the strings in&nbsp;<code>dictionary</code>&nbsp;are <strong>distinct</strong>.</li>
	<li><code>1 &lt;=&nbsp;searchWord.length &lt;= 100</code></li>
	<li><code>searchWord</code>&nbsp;consists of only lower-case English letters.</li>
	<li><code>buildDict</code>&nbsp;will be called only once before <code>search</code>.</li>
	<li>At most <code>100</code> calls will be made to <code>search</code>.</li>
</ul>


**Related Topics**:  
[Hash Table](https://leetcode.com/tag/hash-table/), [String](https://leetcode.com/tag/string/), [Design](https://leetcode.com/tag/design/), [Trie](https://leetcode.com/tag/trie/)

**Similar Questions**:
* [Implement Trie (Prefix Tree) (Medium)](https://leetcode.com/problems/implement-trie-prefix-tree/)
* [Longest Word in Dictionary (Medium)](https://leetcode.com/problems/longest-word-in-dictionary/)

## Solution 1. Trie

```cpp
// OJ: https://leetcode.com/problems/implement-magic-dictionary/
// Author: A M A N
// Time : O()
// Space: O()
class MagicDictionary {
private:
    struct TrieNode {
        bool end;
        TrieNode* children[26] = {nullptr};
        TrieNode(){
            end = false;
        }
    };
    TrieNode* root;
public:
    /** Initialize your data structure here. */
    MagicDictionary() {
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
    void buildDict(vector<string> dictionary) {
        for(string s: dictionary)
            insert(s);
    }
    bool search(string searchWord) {
        return searchUtil(searchWord, root, 0, 0); // to find recursively
    }
    bool searchUtil(string word,TrieNode* node, int start, bool skipped){
        for(int i=start; i<word.size(); i++){
            int idx = word[i] - 'a';
            if(node->children[idx]==nullptr) // the letter doesn't exist in the trie
                if(skipped) // one word has already been skipped 
                    return false;  // so return false
                else { // we have one skip option left, let's use it
                    for(int j=0; j<26; j++) 
                        if(node->children[j]) // using any character that is present in the node as alternative to required char
                            if(searchUtil(word, node->children[j], i+1, true))
                                return true;
                    return false; // the letter doesn't exist and we tried skipping 1 char as well, yet not found so return false
                }
            else if(!skipped){ // this is the tricky part, even though the char exist in the trie
                for(int j=0; j<26; j++) // it might lead to a false result
                    if(node->children[j] and j!=idx) // so let's try skipping this char as well j != idx, if we can skip
                        if(searchUtil(word, node->children[j], i+1, true))
                            return true;
            }
            node = node->children[idx];
        }
        return node->end ? skipped : false; 
    }
};
/**
 * Your MagicDictionary object will be instantiated and called as such:
 * MagicDictionary* obj = new MagicDictionary();
 * obj->buildDict(dictionary);
 * bool param_2 = obj->search(searchWord);
 */
```