# [1268. Search Suggestions System (Medium)](https://leetcode.com/problems/search-suggestions-system/)

<p>Given an array of strings <code>products</code> and a string <code>searchWord</code>. We want to design a system that suggests at most three product names from <code>products</code>&nbsp;after each character of&nbsp;<code>searchWord</code> is typed. Suggested products should have common prefix with the searchWord. If there are&nbsp;more than three products with a common prefix&nbsp;return the three lexicographically minimums products.</p>

<p>Return <em>list of lists</em> of the suggested <code>products</code> after each character of&nbsp;<code>searchWord</code> is typed.&nbsp;</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> products = ["mobile","mouse","moneypot","monitor","mousepad"], searchWord = "mouse"
<strong>Output:</strong> [
["mobile","moneypot","monitor"],
["mobile","moneypot","monitor"],
["mouse","mousepad"],
["mouse","mousepad"],
["mouse","mousepad"]
]
<strong>Explanation:</strong> products sorted lexicographically = ["mobile","moneypot","monitor","mouse","mousepad"]
After typing m and mo all products match and we show user ["mobile","moneypot","monitor"]
After typing mou, mous and mouse the system suggests ["mouse","mousepad"]
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> products = ["havana"], searchWord = "havana"
<strong>Output:</strong> [["havana"],["havana"],["havana"],["havana"],["havana"],["havana"]]
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> products = ["bags","baggage","banner","box","cloths"], searchWord = "bags"
<strong>Output:</strong> [["baggage","bags","banner"],["baggage","bags","banner"],["baggage","bags"],["bags"]]
</pre>

<p><strong>Example 4:</strong></p>

<pre><strong>Input:</strong> products = ["havana"], searchWord = "tatiana"
<strong>Output:</strong> [[],[],[],[],[],[],[]]
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= products.length &lt;= 1000</code></li>
	<li>There are no&nbsp;repeated elements in&nbsp;<code>products</code>.</li>
	<li><code>1 &lt;= Σ products[i].length &lt;= 2 * 10^4</code></li>
	<li>All characters of <code>products[i]</code> are lower-case English letters.</li>
	<li><code>1 &lt;= searchWord.length &lt;= 1000</code></li>
	<li>All characters of <code>searchWord</code>&nbsp;are lower-case English letters.</li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [String](https://leetcode.com/tag/string/), [Trie](https://leetcode.com/tag/trie/)

## Solution 1. Trie

```cpp
// OJ: https://leetcode.com/problems/search-suggestions-system/
// Author: A M A N
// Time : O(M) where M is total number of characters in products
// Space: O(26n) = O(n) where n is the maximum length of a string in products vector
private:
    struct TrieNode{
        bool end;
        TrieNode* children[26]={nullptr};
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
        for(auto c: word){
            int idx = c-'a';
            if(node->children[idx]==nullptr)
                node->children[idx] = new TrieNode();
            node = node->children[idx];
        }
        node->end = true;
    }    
    
    vector<vector<string>> suggest(string word){
        vector<vector<string>> ans;
        string typed;
        auto node = root;
        bool notFound = false;
        for(char c: word){
            if(!notFound){
                typed += c;
                int idx = c-'a';
                if(node->children[idx]==nullptr){
                    notFound = true;
                }else{
                    node = node->children[idx];
                    vector<string> prediction;
                    findThree(node, "", prediction);
                    for(auto &s: prediction)
                        s = typed + s;
                    ans.push_back(prediction);
                }
            }
            if(notFound) ans.push_back(vector<string>());
        }
        return ans;
    }
    
    //O(1)
    void findThree(TrieNode* root, string temp, vector<string>& res){
        if(res.size()>=3)
            return;
        if(root->end)
            res.push_back(temp);
        for(int i=0; i<26; i++){
            if(root->children[i])
                findThree(root->children[i], temp+string(1, i+'a'), res);
        }
    } 
};

class Solution {
public:
    vector<vector<string>> suggestedProducts(vector<string>& products, string searchWord) {
        Trie* t = new Trie();
        //Building the trie would take total no of operations = total no of chars in products
        for(auto s: products)
            t->insert(s);
        return t->suggest(searchWord);        
    }
};
```


## Solution 2. Binary Search

```
In a sorted list of words,
for any word A[i],
all its sugested words must following this word in the list.

For example, if A[i] is a prefix of A[j],
A[i] must be the prefix of A[i + 1], A[i + 2], ..., A[j]
we can binary search the position of each prefix of search word,
and check if the next 3 words is a valid suggestion.

```

```cpp
// OJ: https://leetcode.com/problems/search-suggestions-system/
// Author: A M A N
// Time : O(NlogN)
// Space: O(1)
// Ref  : https://leetcode.com/problems/search-suggestions-system/solution/

class Solution {
public:
    vector<vector<string>> suggestedProducts(vector<string> &products,
                                             string searchWord) {
        sort(products.begin(), products.end());
        vector<vector<string>> result;
        int start, bsStart = 0, n=products.size();
        string prefix;
        for (char &c : searchWord) {
            prefix += c;
            // Get the starting index of word starting with `prefix`.
            start = lower_bound(products.begin() + bsStart, products.end(), prefix) - products.begin();
            // Add empty vector to result.
            result.push_back({});
            // Add the words with the same prefix to the result.
            // Loop runs until `i` reaches the end of input or 3 times or till the
            // prefix is same for `products[i]` Whichever comes first.
            for (int i = start; i < min(start + 3, n) && !products[i].compare(0, prefix.length(), prefix); i++)
                result.back().push_back(products[i]);
            // Reduce the size of elements to binary search on since we know
            bsStart = start;
        }
        return result;
    }
};
```