# [443. String Compression (Medium)](https://leetcode.com/problems/string-compression/)

<p>Given an array of characters <code>chars</code>, compress it using the following algorithm:</p>

<p>Begin with an empty string <code>s</code>. For each group of <strong>consecutive repeating characters</strong> in <code>chars</code>:</p>

<ul>
	<li>If the group's length is <code>1</code>, append the character to <code>s</code>.</li>
	<li>Otherwise, append the character followed by the group's length.</li>
</ul>

<p>The compressed string <code>s</code> <strong>should not be returned separately</strong>, but instead, be stored <strong>in the input character array <code>chars</code></strong>. Note that group lengths that are <code>10</code> or longer will be split into multiple characters in <code>chars</code>.</p>

<p>After you are done <b>modifying the input array</b>, return <em>the new length of the array</em>.</p>
You must write an algorithm that uses only constant extra space.
<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> chars = ["a","a","b","b","c","c","c"]
<strong>Output:</strong> Return 6, and the first 6 characters of the input array should be: ["a","2","b","2","c","3"]
<strong>Explanation:</strong>&nbsp;The groups are "aa", "bb", and "ccc". This compresses to "a2b2c3".
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> chars = ["a"]
<strong>Output:</strong> Return 1, and the first character of the input array should be: ["a"]
<strong>Explanation:</strong>&nbsp;The only group is "a", which remains uncompressed since it's a single character.
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> chars = ["a","b","b","b","b","b","b","b","b","b","b","b","b"]
<strong>Output:</strong> Return 4, and the first 4 characters of the input array should be: ["a","b","1","2"].
<strong>Explanation:</strong>&nbsp;The groups are "a" and "bbbbbbbbbbbb". This compresses to "ab12".</pre>

<p><strong>Example 4:</strong></p>

<pre><strong>Input:</strong> chars = ["a","a","a","b","b","a","a"]
<strong>Output:</strong> Return 6, and the first 6 characters of the input array should be: ["a","3","b","2","a","2"].
<strong>Explanation:</strong>&nbsp;The groups are "aaa", "bb", and "aa". This compresses to "a3b2a2". Note that each group is independent even if two groups have the same character.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= chars.length &lt;= 2000</code></li>
	<li><code>chars[i]</code> is a lowercase English letter, uppercase English letter, digit, or symbol.</li>
</ul>


**Companies**:  
[Microsoft](https://leetcode.com/company/microsoft), [Facebook](https://leetcode.com/company/facebook), [Apple](https://leetcode.com/company/apple), [Goldman Sachs](https://leetcode.com/company/goldman-sachs), [Yandex](https://leetcode.com/company/yandex), [Expedia](https://leetcode.com/company/expedia), [Amazon](https://leetcode.com/company/amazon), [Google](https://leetcode.com/company/google)

**Related Topics**:  
[Two Pointers](https://leetcode.com/tag/two-pointers/), [String](https://leetcode.com/tag/string/)

**Similar Questions**:
* [Count and Say (Medium)](https://leetcode.com/problems/count-and-say/)
* [Encode and Decode Strings (Medium)](https://leetcode.com/problems/encode-and-decode-strings/)
* [Design Compressed String Iterator (Easy)](https://leetcode.com/problems/design-compressed-string-iterator/)
* [Decompress Run-Length Encoded List (Easy)](https://leetcode.com/problems/decompress-run-length-encoded-list/)

## Solution 1. Two-pointers

using extra space

```cpp
// OJ: https://leetcode.com/problems/string-compression/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    int compress(vector<char>& s) {
        vector<char> ans;
        int i=0, j=0;
        while(j<s.size()){
            while(j<s.size() and s[i]==s[j]) j++;
            ans.push_back(s[i]);
            if(j-i+1>2){
                string n = to_string(j-i);
                for(auto c: n)
                    ans.push_back(c);
            }
            i=j;
        }
        s = ans;
        return ans.size();
    }
};
```

## Solution 2. Two-pointers 

Assuming ' ' is not present in the input

```cpp
// OJ: https://leetcode.com/problems/string-compression/
// Author: A M A N
// Time : O()
// Space: O(1)
class Solution {
public:
    int compress(vector<char>& s) {
        int i=0, j=0;
        while(j<s.size()){
            while(j<s.size() and s[i]==s[j]) j++;
            if(j-i+1>2){ 
                string n = to_string(j-i);
                for(auto c: n)
                    s[++i] = c;
                while(i+1<j)
                    s[++i] = ' '; 
            }
            i=j;
        }
        s.erase(remove(s.begin(), s.end(), ' '), s.end());
        return s.size();
    }
};
```

## Solution 3. Two-pointers

Traverse the array using `idx` and finalize the array using another independent pointer `idxAns`

```cpp
// OJ: https://leetcode.com/problems/string-compression/
// Author: A M A N
// Time : O()
// Space: O()
class Solution {
public:
    int compress(vector<char>& s) {
        int idx = 0, idxAns = 0;
        while(idx < s.size()){
            int cnt = 0;
            char curChar = s[idx];
            while(idx<s.size() and s[idx]==curChar) cnt++, idx++;
            s[idxAns++] = curChar;
            if(cnt!=1){
                for(char c: to_string(cnt))
                    s[idxAns++] = c;
            }
        }
        return idxAns;
    }
};
```