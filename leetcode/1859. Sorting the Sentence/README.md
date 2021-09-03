# [1859. Sorting the Sentence (Easy)](https://leetcode.com/problems/sorting-the-sentence/)

<p>A <strong>sentence</strong> is a list of words that are separated by a single space with no leading or trailing spaces. Each word consists of lowercase and uppercase English letters.</p>

<p>A sentence can be <strong>shuffled</strong> by appending the <strong>1-indexed word position</strong> to each word then rearranging the words in the sentence.</p>

<ul>
	<li>For example, the sentence <code>"This is a sentence"</code> can be shuffled as <code>"sentence4 a3 is2 This1"</code> or <code>"is2 sentence4 This1 a3"</code>.</li>
</ul>

<p>Given a <strong>shuffled sentence</strong> <code>s</code> containing no more than <code>9</code> words, reconstruct and return <em>the original sentence</em>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> s = "is2 sentence4 This1 a3"
<strong>Output:</strong> "This is a sentence"
<strong>Explanation:</strong> Sort the words in s to their original positions "This1 is2 a3 sentence4", then remove the numbers.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> s = "Myself2 Me1 I4 and3"
<strong>Output:</strong> "Me Myself and I"
<strong>Explanation:</strong> Sort the words in s to their original positions "Me1 Myself2 and3 I4", then remove the numbers.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>2 &lt;= s.length &lt;= 200</code></li>
	<li><code>s</code> consists of lowercase and uppercase English letters, spaces, and digits from <code>1</code> to <code>9</code>.</li>
	<li>The number of words in <code>s</code> is between <code>1</code> and <code>9</code>.</li>
	<li>The words in <code>s</code> are separated by a single space.</li>
	<li><code>s</code> contains no leading or trailing spaces.</li>
</ul>

**Related Topics**:  
[String](https://leetcode.com/tag/string/), [Sorting](https://leetcode.com/tag/sorting/)

## Solution 1. 

```cpp
// OJ: https://leetcode.com/problems/sorting-the-sentence/
// Author: A M A N
// Time : O()
// Space: O()
class Solution {
public:
    vector<string> breakSentence(string& s){
        vector<string> res;
        string word;
        for(char c: s){
            if(c==' '){
                res.push_back(word);
                word = "";
            }else
                word+=c;
        }
        res.push_back(word);
        return res;
    }
    pair<int, string> findRank(string s){
        int j = s.size()-1;
        while(isdigit(s[j]))
            j--; 
        int num = stoi(s.substr(j+1));
        return {num, s.substr(0, j+1)};
    }
    string sortSentence(string s) {
        vector<string> words = breakSentence(s);
        map<int, string> mp;
        for(auto &s: words){
            auto [rank, word] = findRank(s);
            mp[rank] = word;
        }
        string ans;
        for(auto a: mp)
            ans += a.second, ans += " ";
        return ans.substr(0, ans.size()-1);
    }
};
```