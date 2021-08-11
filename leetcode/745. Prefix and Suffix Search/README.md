# [745. Prefix and Suffix Search (Hard)](https://leetcode.com/problems/prefix-and-suffix-search/)

<p>Design a special dictionary with some words that searchs the words in it by a prefix and a suffix.</p>

<p>Implement the <code>WordFilter</code> class:</p>

<ul>
	<li><code>WordFilter(string[] words)</code> Initializes the object with the <code>words</code> in the dictionary.</li>
	<li><code>f(string prefix, string suffix)</code> Returns <em>the index of the word in the dictionary,</em> which has the prefix <code>prefix</code> and the suffix <code>suffix</code>. If there is more than one valid index, return <strong>the largest</strong> of them. If there is no such word in the dictionary, return <code>-1</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input</strong>
["WordFilter", "f"]
[[["apple"]], ["a", "e"]]
<strong>Output</strong>
[null, 0]

<strong>Explanation</strong>
WordFilter wordFilter = new WordFilter(["apple"]);
wordFilter.f("a", "e"); // return 0, because the word at index 0 has prefix = "a" and suffix = 'e".
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 15000</code></li>
	<li><code>1 &lt;= words[i].length &lt;= 10</code></li>
	<li><code>1 &lt;= prefix.length, suffix.length &lt;= 10</code></li>
	<li><code>words[i]</code>, <code>prefix</code> and <code>suffix</code> consist of lower-case English letters only.</li>
	<li>At most <code>15000</code> calls will be made to the function <code>f</code>.</li>
</ul>


**Related Topics**:  
[String](https://leetcode.com/tag/string/), [Design](https://leetcode.com/tag/design/), [Trie](https://leetcode.com/tag/trie/)

**Similar Questions**:
* [Design Add and Search Words Data Structure (Medium)](https://leetcode.com/problems/design-add-and-search-words-data-structure/)

## Solution 1. Trie

```
Consider the word 'apple'. For each suffix of the word, we could insert that suffix, followed by '#', followed by the word, all into the trie.

For example, we will insert '#apple', 'e#apple', 'le#apple', 'ple#apple', 'pple#apple', 'apple#apple' into the trie. Then for a query like prefix = "ap", suffix = "le", we can find it by querying our trie for le#ap
```

```cpp
// OJ: https://leetcode.com/problems/prefix-and-suffix-search/
// Author: A M A N
// Time : O(NK^2 + QK) where N: number of words, K: maximum length word, Q: queries 
// Space: O(NK^2) the size of the trie
// Ref  : https://leetcode.com/problems/prefix-and-suffix-search/solution/
class WordFilter {
private:
    struct TrieNode{
        bool end = false;
        int index = -1;
        unordered_map<char, TrieNode*> children;
    };
    TrieNode* root = new TrieNode();
public:
    void insert(string &word, int idx){
        auto node = root;
        for(char &c: word){
            if(node->children.find(c)==node->children.end())
                node->children[c] = new TrieNode();
            node = node->children[c];
            node->index = idx;
        }
        node->end = true;
    }
    WordFilter(vector<string>& words) {
        root = new TrieNode();
        for(int i=0; i<words.size(); i++){
            for(int j=0; j<words[i].size(); j++){
                string s = words[i].substr(j) + '#' + words[i];
                insert(s, i);
            }
        }
    }
    int find(string q){
        auto node = root;
        for(char c: q){
            if(node->children.find(c)==node->children.end())
                return -1;
            node = node->children[c];
        }
        return node->index;
    }
    int f(string prefix, string suffix) {
        string query = suffix+'#'+prefix;
        return find(query);
    }
};
/**
 * Your WordFilter object will be instantiated and called as such:
 * WordFilter* obj = new WordFilter(words);
 * int param_1 = obj->f(prefix,suffix);
 */
```