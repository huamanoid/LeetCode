# [953. Verifying an Alien Dictionary (Easy)](https://leetcode.com/problems/verifying-an-alien-dictionary/)

<p>In an alien language, surprisingly, they also use English lowercase letters, but possibly in a different <code>order</code>. The <code>order</code> of the alphabet is some permutation of lowercase letters.</p>

<p>Given a sequence of <code>words</code> written in the alien language, and the <code>order</code> of the alphabet, return <code>true</code> if and only if the given <code>words</code> are sorted lexicographically in this alien language.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> words = ["hello","leetcode"], order = "hlabcdefgijkmnopqrstuvwxyz"
<strong>Output:</strong> true
<strong>Explanation: </strong>As 'h' comes before 'l' in this language, then the sequence is sorted.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> words = ["word","world","row"], order = "worldabcefghijkmnpqstuvxyz"
<strong>Output:</strong> false
<strong>Explanation: </strong>As 'd' comes after 'l' in this language, then words[0] &gt; words[1], hence the sequence is unsorted.
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> words = ["apple","app"], order = "abcdefghijklmnopqrstuvwxyz"
<strong>Output:</strong> false
<strong>Explanation: </strong>The first three characters "app" match, and the second string is shorter (in size.) According to lexicographical rules "apple" &gt; "app", because 'l' &gt; '∅', where '∅' is defined as the blank character which is less than any other character (<a href="https://en.wikipedia.org/wiki/Lexicographical_order" target="_blank">More info</a>).
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 100</code></li>
	<li><code>1 &lt;= words[i].length &lt;= 20</code></li>
	<li><code>order.length == 26</code></li>
	<li>All characters in <code>words[i]</code> and <code>order</code> are English lowercase letters.</li>
</ul>


**Companies**:  
[Facebook](https://leetcode.com/company/facebook), [Uber](https://leetcode.com/company/uber), [eBay](https://leetcode.com/company/ebay), [DE Shaw](https://leetcode.com/company/de-shaw)

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Hash Table](https://leetcode.com/tag/hash-table/), [String](https://leetcode.com/tag/string/)
 
## Solution 1. Sorting 

```cpp
// OJ: https://leetcode.com/problems/verifying-an-alien-dictionary/
// Author: A M A N
// Time : O(NlogN * L) where L is the average length of words
// Space: O(N * L)
class Solution {
public:
    bool isAlienSorted(vector<string>& words, string order) {
        vector<string> copy = words;
        unordered_map<char, int> O;
        for(int i=0; i<order.size(); i++)
            O[order[i]] = i;

        sort(copy.begin(), copy.end(), [&](const string &a, const string &b){
            for(int i=0; i<min((int)a.size(), (int)b.size()); i++){
                if(a[i]==b[i]) continue;
                return O[a[i]] < O[b[i]];
            }
            return a.size() < b.size();
        });
        return copy==words; // is sorted or not
    }
};
```

## Solution 2. 

```cpp
// OJ: https://leetcode.com/problems/verifying-an-alien-dictionary/
// Author: A M A N
// Time : O(N*L) where L is the average length of words | Total no. of chars
// Space: O(1)
class Solution {
public:
    bool isAlienSorted(vector<string>& words, string order) {
        unordered_map<char, int> O;
        for(int i=0; i<order.size(); i++)
            O[order[i]] = i;
        for(int i=0; i+1<words.size(); i++){
            bool less = false;
            for(int j=0; j<min(words[i].size(), words[i+1].size()); j++){
                if(O[words[i][j]] < O[words[i+1][j]]) 
                    less = true;
                else if(!less and O[words[i][j]] > O[words[i+1][j]])
                    return false;
            }
            if(!less and words[i].size() > words[i+1].size())
                return false;
        }
        return true;
    }
};
```