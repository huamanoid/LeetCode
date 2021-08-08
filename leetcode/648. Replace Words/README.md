# [648. Replace Words (Medium)](https://leetcode.com/problems/replace-words/)

<p>In English, we have a concept called <strong>root</strong>, which can be followed by some other word&nbsp;to form another longer word - let's call this word <strong>successor</strong>. For example, when the <strong>root</strong> <code>"an"</code> is&nbsp;followed by the <strong>successor</strong>&nbsp;word&nbsp;<code>"other"</code>, we&nbsp;can form a new word <code>"another"</code>.</p>

<p>Given a <code>dictionary</code> consisting of many <strong>roots</strong> and a <code>sentence</code>&nbsp;consisting of words separated by spaces, replace all the <strong>successors</strong> in the sentence with the <strong>root</strong> forming it. If a <strong>successor</strong> can be replaced by more than one <strong>root</strong>,&nbsp;replace it with the <strong>root</strong> that has&nbsp;<strong>the shortest length</strong>.</p>

<p>Return <em>the <code>sentence</code></em> after the replacement.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<pre><strong>Input:</strong> dictionary = ["cat","bat","rat"], sentence = "the cattle was rattled by the battery"
<strong>Output:</strong> "the cat was rat by the bat"
</pre><p><strong>Example 2:</strong></p>
<pre><strong>Input:</strong> dictionary = ["a","b","c"], sentence = "aadsfasf absbs bbab cadsfafs"
<strong>Output:</strong> "a a b c"
</pre><p><strong>Example 3:</strong></p>
<pre><strong>Input:</strong> dictionary = ["a", "aa", "aaa", "aaaa"], sentence = "a aa a aaaa aaa aaa aaa aaaaaa bbb baba ababa"
<strong>Output:</strong> "a a a a a a a a bbb baba a"
</pre><p><strong>Example 4:</strong></p>
<pre><strong>Input:</strong> dictionary = ["catt","cat","bat","rat"], sentence = "the cattle was rattled by the battery"
<strong>Output:</strong> "the cat was rat by the bat"
</pre><p><strong>Example 5:</strong></p>
<pre><strong>Input:</strong> dictionary = ["ac","ab"], sentence = "it is abnormal that this solution is accepted"
<strong>Output:</strong> "it is ab that this solution is ac"
</pre>
<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= dictionary.length&nbsp;&lt;= 1000</code></li>
	<li><code>1 &lt;= dictionary[i].length &lt;= 100</code></li>
	<li><code>dictionary[i]</code>&nbsp;consists of only lower-case letters.</li>
	<li><code>1 &lt;= sentence.length &lt;= 10^6</code></li>
	<li><code>sentence</code>&nbsp;consists of only lower-case letters and spaces.</li>
	<li>The number of words in&nbsp;<code>sentence</code>&nbsp;is in the range <code>[1, 1000]</code></li>
	<li>The length of each word in&nbsp;<code>sentence</code>&nbsp;is in the range <code>[1, 1000]</code></li>
	<li>Each two consecutive words in&nbsp;<code>sentence</code>&nbsp;will be separated by exactly one space.</li>
	<li><code>sentence</code>&nbsp;does not have leading or trailing spaces.</li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Hash Table](https://leetcode.com/tag/hash-table/), [String](https://leetcode.com/tag/string/), [Trie](https://leetcode.com/tag/trie/)

**Similar Questions**:
* [Implement Trie (Prefix Tree) (Medium)](https://leetcode.com/problems/implement-trie-prefix-tree/)

## Solution 1. Trie

```cpp
// OJ: https://leetcode.com/problems/replace-words/
// Author: A M A N
// Time : O()
// Space: O()
class Solution {
public:
    struct TrieNode{
        bool end;
        TrieNode* children[26] = {nullptr};
        //Constructor
        TrieNode(){
            end = false;
        }
    };
    class Trie{
    private:
        TrieNode* root;
    public:
        //Constructor
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
        string findCommon(string word){
            auto node = root;
            string common="";
            for(char c: word){
                int idx = c-'a';
                if(node->end)
                    return common;
                if(node->children[idx]==nullptr)
                    return "";
                else
                    common+=c;
                node = node->children[idx];
            }
            return "";
        }
    };
    vector<string> breakSentence(string s){
        vector<string> res;
        for(int i=0; i<s.size(); i++){
            int j = i;
            string word;
            while(j<s.size() and s[j]!=' ')word+=s[j++];
            res.push_back(word);
            i = j;
        }
        return res;
    }
    string replaceWords(vector<string>& dictionary, string sentence) {
        Trie* t = new Trie();
        for(string s: dictionary)
            t->insert(s);
        string res = "";
        vector<string> words = breakSentence(sentence);
        for(string word: words){
            string newWord = t->findCommon(word);
            res += (newWord==""?word: newWord);
            res += " ";
        }
        return res.substr(0, res.size()-1);
    }
};
```