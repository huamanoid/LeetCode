# [771. Jewels and Stones (Easy)](https://leetcode.com/problems/jewels-and-stones/)

<p>You're given strings <code>jewels</code> representing the types of stones that are jewels, and <code>stones</code> representing the stones you have. Each character in <code>stones</code> is a type of stone you have. You want to know how many of the stones you have are also jewels.</p>

<p>Letters are case sensitive, so <code>"a"</code> is considered a different type of stone from <code>"A"</code>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<pre><strong>Input:</strong> jewels = "aA", stones = "aAAbbbb"
<strong>Output:</strong> 3
</pre><p><strong>Example 2:</strong></p>
<pre><strong>Input:</strong> jewels = "z", stones = "ZZ"
<strong>Output:</strong> 0
</pre>
<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;=&nbsp;jewels.length, stones.length &lt;= 50</code></li>
	<li><code>jewels</code> and <code>stones</code> consist of only English letters.</li>
	<li>All the characters of&nbsp;<code>jewels</code> are <strong>unique</strong>.</li>
</ul>


**Companies**:  
[Amazon](https://leetcode.com/company/amazon)

**Related Topics**:  
[Hash Table](https://leetcode.com/tag/hash-table/), [String](https://leetcode.com/tag/string/)

## Solution 1. Hashing

```cpp
// OJ: https://leetcode.com/problems/jewels-and-stones/
// Author: A M A N
// Time : O(N+M)
// Space: O(M)
class Solution {
public:
    int numJewelsInStones(string jewels, string stones) {
        int ans = 0;
        unordered_set<char> sett;
        for(char c: jewels) sett.insert(c);
        for(char c: stones)
            if(sett.count(c))
                ans++;
        return ans;
    }
};
```