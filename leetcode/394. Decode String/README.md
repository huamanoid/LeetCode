# [394. Decode String (Medium)](https://leetcode.com/problems/decode-string/)

<p>Given an encoded string, return its decoded string.</p>

<p>The encoding rule is: <code>k[encoded_string]</code>, where the <code>encoded_string</code> inside the square brackets is being repeated exactly <code>k</code> times. Note that <code>k</code> is guaranteed to be a positive integer.</p>

<p>You may assume that the input string is always valid; No extra white spaces, square brackets are well-formed, etc.</p>

<p>Furthermore, you may assume that the original data does not contain any digits and that digits are only for those repeat numbers, <code>k</code>. For example, there won't be input like <code>3a</code> or <code>2[4]</code>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<pre><strong>Input:</strong> s = "3[a]2[bc]"
<strong>Output:</strong> "aaabcbc"
</pre><p><strong>Example 2:</strong></p>
<pre><strong>Input:</strong> s = "3[a2[c]]"
<strong>Output:</strong> "accaccacc"
</pre><p><strong>Example 3:</strong></p>
<pre><strong>Input:</strong> s = "2[abc]3[cd]ef"
<strong>Output:</strong> "abcabccdcdcdef"
</pre><p><strong>Example 4:</strong></p>
<pre><strong>Input:</strong> s = "abc3[cd]xyz"
<strong>Output:</strong> "abccdcdcdxyz"
</pre>
<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 30</code></li>
	<li><code>s</code> consists of lowercase English letters, digits, and square brackets <code>'[]'</code>.</li>
	<li><code>s</code> is guaranteed to be <strong>a valid</strong> input.</li>
	<li>All the integers in <code>s</code> are in the range <code>[1, 300]</code>.</li>
</ul>


**Related Topics**:  
[String](https://leetcode.com/tag/string/), [Stack](https://leetcode.com/tag/stack/), [Recursion](https://leetcode.com/tag/recursion/)

**Similar Questions**:
* [Encode String with Shortest Length (Hard)](https://leetcode.com/problems/encode-string-with-shortest-length/)
* [Number of Atoms (Hard)](https://leetcode.com/problems/number-of-atoms/)
* [Brace Expansion (Medium)](https://leetcode.com/problems/brace-expansion/)

## Solution 1. DFS

```cpp
// OJ: https://leetcode.com/problems/decode-string/
// Author: A M A N
// Time : O(max(K) * N )
// Space: O()
class Solution {
public:
    int findClosingPair(string s, int start, int end){
        stack<int>st;
        for(int i=start; i<=end; i++){
            if(s[i]=='[')
                st.push(1);
            else if(s[i]==']')
                st.pop();
            if(st.size()==0)
                return i;
        }
        return -1;
    }
    string dfs(string s, int start, int end){
        if(start>end)
            return "";
        string ans = "";
        for(int i=start; i<=end; i++){
            if(s[i]>='0' and s[i]<='9'){
                int j = i;
                int num=0;
                while(j<s.size() and s[j]>='0' and s[j]<='9')
                    num = num*10 + s[j]-'0', j++;
                int start_new = j; // index of '['
                int end_new   = findClosingPair(s, j, end); //returns index of ']'
                string temp = dfs(s, start_new+1, end_new-1);
                while(num-- >0) ans += temp; // adding the string inside the [] the freq no of the times
                i = end_new;    
            }else{
                ans+=s[i];
            }
        }
        return ans;
    }
    string decodeString(string s) {
        return dfs(s, 0, s.size()-1);
    }
};
```