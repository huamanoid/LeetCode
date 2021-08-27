# [1309. Decrypt String from Alphabet to Integer Mapping (Easy)](https://leetcode.com/problems/decrypt-string-from-alphabet-to-integer-mapping/)

<p>Given a string <code>s</code> formed by digits (<code>'0'</code> - <code>'9'</code>)&nbsp;and <code>'#'</code>&nbsp;.&nbsp;We want to map <code>s</code> to English lowercase characters as follows:</p>

<ul>
	<li>Characters (<code>'a'</code> to <code>'i')</code> are&nbsp;represented by&nbsp;(<code>'1'</code> to&nbsp;<code>'9'</code>)&nbsp;respectively.</li>
	<li>Characters (<code>'j'</code> to <code>'z')</code> are represented by (<code>'10#'</code>&nbsp;to&nbsp;<code>'26#'</code>)&nbsp;respectively.&nbsp;</li>
</ul>

<p>Return the string formed after mapping.</p>

<p>It's guaranteed that a unique mapping will always exist.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> s = "10#11#12"
<strong>Output:</strong> "jkab"
<strong>Explanation:</strong> "j" -&gt; "10#" , "k" -&gt; "11#" , "a" -&gt; "1" , "b" -&gt; "2".
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> s = "1326#"
<strong>Output:</strong> "acz"
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> s = "25#"
<strong>Output:</strong> "y"
</pre>

<p><strong>Example 4:</strong></p>

<pre><strong>Input:</strong> s = "12345678910#11#12#13#14#15#16#17#18#19#20#21#22#23#24#25#26#"
<strong>Output:</strong> "abcdefghijklmnopqrstuvwxyz"
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 1000</code></li>
	<li><code>s[i]</code> only contains digits letters (<code>'0'</code>-<code>'9'</code>) and <code>'#'</code>&nbsp;letter.</li>
	<li><code>s</code> will be valid string&nbsp;such that mapping is always possible.</li>
</ul>


**Companies**:  
[Amazon](https://leetcode.com/company/amazon), [Quip (Salesforce)](https://leetcode.com/company/quip)

**Related Topics**:  
[String](https://leetcode.com/tag/string/)

## Solution 1.

```cpp
// OJ: https://leetcode.com/problems/decrypt-string-from-alphabet-to-integer-mapping/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    string freqAlphabets(string s) {
        reverse(s.begin(), s.end()); // to be sure when to look for two digits and when for one 
        string temp, ans;
        for(int i=0; i<s.size(); i++){
            if(s[i]=='#'){ // next two digits are of alphabet
                temp = s.substr(i+1, 2);
                temp =(temp[1]-'0')*10+(temp[0]-'0')+'a'-1;
                i+=2;
            }else{ // current digits is a alphabet
                temp = (s[i]-'0')+'a'-1;
            }
            ans+= temp;
        }
        reverse(ans.begin(), ans.end());
        return ans;
    }
};
```