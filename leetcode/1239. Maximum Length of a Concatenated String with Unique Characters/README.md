# [1239. Maximum Length of a Concatenated String with Unique Characters (Medium)](https://leetcode.com/problems/maximum-length-of-a-concatenated-string-with-unique-characters/)

<p>Given an array of strings <code>arr</code>. String <code>s</code> is a concatenation of a sub-sequence of <code>arr</code> which have <strong>unique characters</strong>.</p>

<p>Return <em>the maximum possible length</em> of <code>s</code>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> arr = ["un","iq","ue"]
<strong>Output:</strong> 4
<strong>Explanation:</strong> All possible concatenations are "","un","iq","ue","uniq" and "ique".
Maximum length is 4.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> arr = ["cha","r","act","ers"]
<strong>Output:</strong> 6
<strong>Explanation:</strong> Possible solutions are "chaers" and "acters".
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> arr = ["abcdefghijklmnopqrstuvwxyz"]
<strong>Output:</strong> 26
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= arr.length &lt;= 16</code></li>
	<li><code>1 &lt;= arr[i].length &lt;= 26</code></li>
	<li><code>arr[i]</code> contains only lower case English letters.</li>
</ul>


**Companies**:  
[Tesla](https://leetcode.com/company/tesla), [Microsoft](https://leetcode.com/company/microsoft), [DiDi](https://leetcode.com/company/didi), [American Express](https://leetcode.com/company/american-express)

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [String](https://leetcode.com/tag/string/), [Backtracking](https://leetcode.com/tag/backtracking/), [Bit Manipulation](https://leetcode.com/tag/bit-manipulation/)

## Solution 1. Bit Manipulation (Brute-force)

Trying out all possible combinations.

```cpp
// OJ: https://leetcode.com/problems/maximum-length-of-a-concatenated-string-with-unique-characters/
// Author: A M A N
// Time : O(2^(N) * N * L)
// Space: O()
class Solution {
public:
    bool hasUniqChars(string s){
        vector<int> freq(26, 0);
        for(char c: s) freq[c-'a']++;
        for(int i=0; i<26; i++)
            if(freq[i]>1)
                return false;
        return true;
    }
    int maxLength(vector<string>& vec) {
        vector<string> arr; 
        for(auto s: vec)
            if(hasUniqChars(s))
                arr.push_back(s);
        int n = arr.size();
        int N = 1<<n;
        int maxLength = 0;
        for(int p=0; p<N; p++){
            bool can = true;
            unordered_set<char> sett;
            for(int j=0; j<n and can; j++){
                if(p&(1<<j)){
                    for(char c: arr[j]){
                        if(sett.count(c))
                            can = false;
                    }
                    if(can){
                        for(char c: arr[j]) sett.insert(c);   
                        maxLength = max(maxLength, (int)sett.size());
                    }
                }
            }
        }
        return maxLength;
    }
};
```

## Solution 2. Backtracking

```cpp
// OJ: https://leetcode.com/problems/maximum-length-of-a-concatenated-string-with-unique-characters/
// Author: A M A N
// Time : O(2^N * L)
// Space: O()
class Solution {
public:
    bool hasUniqChars(string s){
        vector<int> freq(26, 0);
        for(char c: s) freq[c-'a']++;
        for(int i=0; i<26; i++)
            if(freq[i]>1)
                return false;
        return true;
    }
    int ans = 0;
    void solve(vector<string>& arr, int idx, set<char> temp){
        if(idx>=arr.size()){
            ans = max(ans, (int)temp.size());
            return;
        }
        //skip
        solve(arr, idx+1, temp);
        //pick if you can
        for(auto c: arr[idx])
            if(temp.count(c))
                return;
        for(auto c: arr[idx])
            temp.insert(c);
        solve(arr, idx+1, temp);
    }
    int maxLength(vector<string>& arr) {
        set<char> temp;
        vector<string> v;
        for(auto s: arr)
            if(hasUniqChars(s))
                v.push_back(s);
        solve(v, 0, temp);
        return ans;
     }
};
````