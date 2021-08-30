# [1647. Minimum Deletions to Make Character Frequencies Unique (Medium)](https://leetcode.com/problems/minimum-deletions-to-make-character-frequencies-unique/)

<p>A string <code>s</code> is called <strong>good</strong> if there are no two different characters in <code>s</code> that have the same <strong>frequency</strong>.</p>

<p>Given a string <code>s</code>, return<em> the <strong>minimum</strong> number of characters you need to delete to make </em><code>s</code><em> <strong>good</strong>.</em></p>

<p>The <strong>frequency</strong> of a character in a string is the number of times it appears in the string. For example, in the string <code>"aab"</code>, the <strong>frequency</strong> of <code>'a'</code> is <code>2</code>, while the <strong>frequency</strong> of <code>'b'</code> is <code>1</code>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> s = "aab"
<strong>Output:</strong> 0
<strong>Explanation:</strong> <code>s</code> is already good.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> s = "aaabbbcc"
<strong>Output:</strong> 2
<strong>Explanation:</strong> You can delete two 'b's resulting in the good string "aaabcc".
Another way it to delete one 'b' and one 'c' resulting in the good string "aaabbc".</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> s = "ceabaacb"
<strong>Output:</strong> 2
<strong>Explanation:</strong> You can delete both 'c's resulting in the good string "eabaab".
Note that we only care about characters that are still in the string at the end (i.e. frequency of 0 is ignored).
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code>&nbsp;contains only lowercase English letters.</li>
</ul>


**Companies**:  
[Microsoft](https://leetcode.com/company/microsoft), [Apple](https://leetcode.com/company/apple), [Tesla](https://leetcode.com/company/tesla)

**Related Topics**:  
[String](https://leetcode.com/tag/string/), [Greedy](https://leetcode.com/tag/greedy/), [Sorting](https://leetcode.com/tag/sorting/)

## Solution 1. Greedy

```cpp
// OJ: https://leetcode.com/problems/minimum-deletions-to-make-character-frequencies-unique/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    int minDeletions(string s) {
        unordered_map<char, int> mp;
        for(char c: s) mp[c]++;
        vector<int> v;
        for(auto [a, f]: mp) v.push_back(f);
        sort(v.begin(), v.end(), greater<int>()); // sorting inorder to skip the first one, amongst the same freq chars
        unordered_set<int> sett(v.begin(), v.end()); // to keep track of used freq
        int cnt=0;
        for(int i=0; i<v.size(); i++){
            int j=i;
            while(j<v.size() and v[i]==v[j])j++; // traversing similar freqs
            for(int k=i+1; k<j; k++){ // skipping the first of all the similar freqs
                while(sett.count(v[k]))v[k]--, cnt++; 
                if(v[k])sett.insert(v[k]); //ignoring the case when the freq is reduces to zero
            }
            i = j-1;
        }
        return cnt;
    }
};
```