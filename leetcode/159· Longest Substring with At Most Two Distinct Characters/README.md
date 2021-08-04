# [159. Longest Substring with At Most Two Distinct Characters (Medium)](https://leetcode.com/problems/longest-substring-with-at-most-two-distinct-characters/)

<p>Given a string, find the length of the longest substring T that contains at most <code>2</code> distinct characters.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> s = “eceba”
<strong>Output:</strong> 3
<strong>Explanation:</strong> T is "ece" which its length is 3.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> s = “aaa”
<strong>Output:</strong> 3
</pre>


<p>&nbsp;</p>


## Solution 1. Sliding Window

```cpp
// OJ: https://leetcode.com/problems/longest-substring-without-repeating-characters/
// Author: A M A N
// Time : O(N)
// Space: O(1)  
class Solution {
public:
    int lengthOfLongestSubstringTwoDistinct(string &s) {
        unordered_map<char, int> mp;
        int ans = 0;
        int i=0, j=0;
        while(j<s.size()){
            mp[s[j]]++;
            if(mp.size()<=2){
                ans = max(ans, j-i+1);
            }else {
                while(mp.size()>2){
                    mp[s[i]]--;
                    if(mp[s[i]]==0)mp.erase(s[i]);
                    i++;
                }
            }
            j++;
        }
        return ans;
    }
};
```