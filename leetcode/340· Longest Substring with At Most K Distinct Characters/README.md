# [159. Longest Substring with At Most Two Distinct Characters (Medium)](https://leetcode.com/problems/longest-substring-with-at-most-k-distinct-characters/)

<p>Given a string S, find the length of the longest substring T that contains at most k distinct characters.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> S = "eceba" and k = 3
<strong>Output:</strong> 4
<strong>Explanation:</strong> T = "eceb"
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong>S = "WORLD" and k = 4
<strong>Output:</strong> 4
<strong>Explanation:</strong> T = "WORL" or "ORLD"
</pre>


<p>&nbsp;</p>


## Solution 1. Sliding Window

```cpp
// OJ: https://www.lintcode.com/problem/386/
// Author: A M A N
// Time : O(N)
// Space: O(1)  
class Solution {
public:
    int lengthOfLongestSubstringKDistinct(string &s, int k) {
       unordered_map<char, int> mp;
        int ans = 0;
        int i=0, j=0;
        while(j<s.size()){
            mp[s[j]]++;
            if(mp.size()<=k){
                ans = max(ans, j-i+1);
            }else {
                while(mp.size()>k){
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