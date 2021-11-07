# [395. Longest Substring with At Least K Repeating Characters (Medium)](https://leetcode.com/problems/longest-substring-with-at-least-k-repeating-characters/)

<p>Given a string <code>s</code> and an integer <code>k</code>, return <em>the length of the longest substring of</em> <code>s</code> <em>such that the frequency of each character in this substring is greater than or equal to</em> <code>k</code>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> s = "aaabb", k = 3
<strong>Output:</strong> 3
<strong>Explanation:</strong> The longest substring is "aaa", as 'a' is repeated 3 times.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> s = "ababbc", k = 2
<strong>Output:</strong> 5
<strong>Explanation:</strong> The longest substring is "ababb", as 'a' is repeated 2 times and 'b' is repeated 3 times.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>4</sup></code></li>
	<li><code>s</code> consists of only lowercase English letters.</li>
	<li><code>1 &lt;= k &lt;= 10<sup>5</sup></code></li>
</ul>


**Related Topics**:  
[Hash Table](https://leetcode.com/tag/hash-table/), [String](https://leetcode.com/tag/string/), [Divide and Conquer](https://leetcode.com/tag/divide-and-conquer/), [Sliding Window](https://leetcode.com/tag/sliding-window/)

## Solution 1. Sliding Window

```cpp
// OJ: https://leetcode.com/problems/longest-substring-with-at-least-k-repeating-characters/
// Author: A M A N
// Time : O(N)
// Space: O(1)
class Solution {
public:
    int longestSubstring(string s, int k) {
        int ans = 0;
// find all sub-strings which have h = [1, 26] unique chars and each char repeats at least k times
// we need this extra constraint to solve the case of shrinking the window
        for(int h = 1; h <=26; h++){
            unordered_map<char, int> mp;
            int i=0, j=0, uniqueChars = 0, validChars = 0;
            while(j<s.size()){
                    if(uniqueChars <= h){ // if # unique Chars is less than the upper limit, 
                        //we can EXTEND the window as we need to find the LONGEST substring
                    mp[s[j]]++;
                    if(mp[s[j]]==1) uniqueChars++;
                    if(mp[s[j]]==k) validChars++; // signifies # chars which occurs more than equal to k times
                    // window which has "h" unique chars and all h occurs at least k times
                    if(uniqueChars==h and uniqueChars==validChars) 
                        ans = max(ans, j-i+1);
                    j++; // extend the window
                } else { // we have more # of unique chars than the upper limit, shrink the window
                    mp[s[i]]--;
                    if(mp[s[i]]==0)   uniqueChars--;
                    if(mp[s[i]]==k-1) validChars--;
                    // window which has "h" unique chars and all h occurs at least k times
                    if(uniqueChars==h and uniqueChars==validChars) 
                        ans = max(ans, j-i+1);
                    i++; // shrink the window
                }
            }
        }
        return ans;
    }
};
```

## Solution 2. Divide & Conquer

```cpp
// OJ: https://leetcode.com/problems/longest-substring-with-at-least-k-repeating-characters/
// Author: A M A N
// Time : O(N^2)
// Space: O(N)
class Solution {
public:
    int solve(string s, int k){
        map<char, int> freq;
        for(char c: s)
            freq[c]++;
        bool can = true;
        for(auto [c, f]: freq)
            if(f<k) 
                can = false;
        if(can)
           return s.size();

           
        int res = 0;
        string temp;
        for(char c: s){
            if(freq[c]<k){
                res = max(res, solve(temp, k));
                temp.clear();
            }else{
                temp+=c;
            }
        }
        res = max(res, solve(temp, k));
        return res;
    }

    int longestSubstring(string s, int k) {
        return solve(s, k);
    }
};
```
