# [954. Array of Doubled Pairs (Medium)](https://leetcode.com/problems/array-of-doubled-pairs/)

<p>Given an array of integers <code>arr</code> of even length, return <code>true</code> if and only if it is possible to reorder it such that <code>arr[2 * i + 1] = 2 * arr[2 * i]</code> for every <code>0 &lt;= i &lt; len(arr) / 2</code>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> arr = [3,1,3,6]
<strong>Output:</strong> false
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> arr = [2,1,2,6]
<strong>Output:</strong> false
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> arr = [4,-2,2,-4]
<strong>Output:</strong> true
<strong>Explanation:</strong> We can take two groups, [-2,-4] and [2,4] to form [-2,-4,2,4] or [2,4,-2,-4].
</pre>

<p><strong>Example 4:</strong></p>

<pre><strong>Input:</strong> arr = [1,2,4,16,8,4]
<strong>Output:</strong> false
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>0 &lt;= arr.length &lt;= 3 * 10<sup>4</sup></code></li>
	<li><code>arr.length</code> is even.</li>
	<li><code>-10<sup>5</sup> &lt;= arr[i] &lt;= 10<sup>5</sup></code></li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Hash Table](https://leetcode.com/tag/hash-table/), [Greedy](https://leetcode.com/tag/greedy/), [Sorting](https://leetcode.com/tag/sorting/)

## Solution 1. Greedy

```cpp
// OJ: https://leetcode.com/problems/array-of-doubled-pairs/
// Author: A M A N
// Time : O(NlogN)
// Space: O(N)
class Solution {
public:
    bool canReorderDoubled(vector<int>& arr) {
        vector<int>pos, neg;
        for(auto a: arr)
            a>=0?pos.push_back(a):neg.push_back(a);
        sort(pos.begin(), pos.end(), greater<int>());
        sort(neg.begin(), neg.end());
        if(pos.size()&1 or neg.size()&1) return false;
        unordered_map<int, int> mp;
        for(auto e: pos){
            if(mp.find(e*2)!=mp.end()){
                mp[e*2]--;
                if(mp[e*2]==0)mp.erase(2*e);
            }else{
                mp[e]++;                
            }
        }
        if(mp.size())return false;
        for(auto e: neg){
            if(mp.find(e*2)!=mp.end()){
                mp[e*2]--;
                if(mp[e*2]==0)mp.erase(2*e);
            }else{
                mp[e]++;                
            }
        }
        if(mp.size())return false;
        return true;
    }
};
```