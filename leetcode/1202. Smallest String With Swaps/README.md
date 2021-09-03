# [1202. Smallest String With Swaps (Medium)](https://leetcode.com/problems/smallest-string-with-swaps/)

<p>You are given a string <code>s</code>, and an array of pairs of indices in the string&nbsp;<code>pairs</code>&nbsp;where&nbsp;<code>pairs[i] =&nbsp;[a, b]</code>&nbsp;indicates 2 indices(0-indexed) of the string.</p>

<p>You can&nbsp;swap the characters at any pair of indices in the given&nbsp;<code>pairs</code>&nbsp;<strong>any number of times</strong>.</p>

<p>Return the&nbsp;lexicographically smallest string that <code>s</code>&nbsp;can be changed to after using the swaps.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> s = "dcab", pairs = [[0,3],[1,2]]
<strong>Output:</strong> "bacd"
<strong>Explaination:</strong> 
Swap s[0] and s[3], s = "bcad"
Swap s[1] and s[2], s = "bacd"
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> s = "dcab", pairs = [[0,3],[1,2],[0,2]]
<strong>Output:</strong> "abcd"
<strong>Explaination: </strong>
Swap s[0] and s[3], s = "bcad"
Swap s[0] and s[2], s = "acbd"
Swap s[1] and s[2], s = "abcd"</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> s = "cba", pairs = [[0,1],[1,2]]
<strong>Output:</strong> "abc"
<strong>Explaination: </strong>
Swap s[0] and s[1], s = "bca"
Swap s[1] and s[2], s = "bac"
Swap s[0] and s[1], s = "abc"
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10^5</code></li>
	<li><code>0 &lt;= pairs.length &lt;= 10^5</code></li>
	<li><code>0 &lt;= pairs[i][0], pairs[i][1] &lt;&nbsp;s.length</code></li>
	<li><code>s</code>&nbsp;only contains lower case English letters.</li>
</ul>


**Related Topics**:  
[Hash Table](https://leetcode.com/tag/hash-table/), [String](https://leetcode.com/tag/string/), [Depth-First Search](https://leetcode.com/tag/depth-first-search/), [Breadth-First Search](https://leetcode.com/tag/breadth-first-search/), [Union Find](https://leetcode.com/tag/union-find/)

**Similar Questions**:
* [Minimize Hamming Distance After Swap Operations (Medium)](https://leetcode.com/problems/minimize-hamming-distance-after-swap-operations/)

## Solution 1. Union-Find

```cpp
// OJ: https://leetcode.com/problems/smallest-string-with-swaps/
// Author: A M A N
// Time : O(N*logN)
// Space: O(N)
class Solution {
public:
    unordered_map<int, int> p;
    int find(int u){
        if(p.find(u)==p.end())
            p[u] = u;
        while(p[u]!=u){
            p[u] = p[p[u]]; //path compression
            u = p[u];
        }
        return u;
    }
    string smallestStringWithSwaps(string s, vector<vector<int>>& pairs) {
        // 1. unite all linked
        for(auto pair: pairs){
            int u = find(pair[0]);
            int v = find(pair[1]);
            if(u>v) p[u] = v;
            else    p[v] = u;
        }
        // 2. sort whose parent is same;
        unordered_map<int, vector<char>> mp;
        vector<vector<int>>pos(s.size());
        for(int i=0; i<s.size(); i++){
            int par = find(i);
            mp[par].push_back(s[i]);
            pos[par].push_back(i);
        }        
        string ans(s.size(), '0');
        // 3. place back the sorted in their respective position;
        for(auto a: mp){
            int par   = a.first;
            sort(a.second.begin(), a.second.end());
            vector<char> temp = a.second;
            for(int i=0; i<temp.size(); i++)
                ans[pos[par][i]] = temp[i];
        }
        return ans;
    }
};
```