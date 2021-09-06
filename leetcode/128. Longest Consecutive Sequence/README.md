# [128. Longest Consecutive Sequence (Medium)](https://leetcode.com/problems/longest-consecutive-sequence/)

<p>Given an unsorted array of integers <code>nums</code>, return <em>the length of the longest consecutive elements sequence.</em></p>

<p>You must write an algorithm that runs in&nbsp;<code>O(n)</code>&nbsp;time.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> nums = [100,4,200,1,3,2]
<strong>Output:</strong> 4
<strong>Explanation:</strong> The longest consecutive elements sequence is <code>[1, 2, 3, 4]</code>. Therefore its length is 4.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> nums = [0,3,7,2,5,8,4,6,0,1]
<strong>Output:</strong> 9
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>0 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>9</sup> &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>


**Companies**:  
[Google](https://leetcode.com/company/google), [Microsoft](https://leetcode.com/company/microsoft), [Amazon](https://leetcode.com/company/amazon), [Facebook](https://leetcode.com/company/facebook), [Adobe](https://leetcode.com/company/adobe), [Twitter](https://leetcode.com/company/twitter), [Qualtrics](https://leetcode.com/company/qualtrics)

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Hash Table](https://leetcode.com/tag/hash-table/), [Union Find](https://leetcode.com/tag/union-find/)

**Similar Questions**:
* [Binary Tree Longest Consecutive Sequence (Medium)](https://leetcode.com/problems/binary-tree-longest-consecutive-sequence/)


## Solution 1. Hashing

```cpp
// OJ: https://leetcode.com/problems/longest-consecutive-sequence/solution/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        int mx=0;
        unordered_set<int>sett(nums.begin(), nums.end());
        for(auto n: nums){
            if(sett.count(n-1))continue; // starts the search from the least num in each sequence only
            int cnt = 1;
            while(sett.count(n+1))
                cnt++, n++;
            mx = max(mx, cnt);
        }
        return mx;
    }
};
```




## Solution 2. Disjoint-set Union

```cpp
// OJ: https://leetcode.com/problems/longest-consecutive-sequence/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    const static int mxN = 1e5+1;
    int p[mxN], sz[mxN];
    unordered_map<int, int> idx;    
    int find(int u){
        while(p[u]!=u){
            p[u] = p[p[u]]; // path compression
            u = p[u];
        }
        return p[u];
    }
    void unite(int u, int v){
        u = find(u), v = find(v);
        if(u!=v){
            // point the root of the smaller tree to the bigger tree's root. 
            // to constraint the maximum depth of any node to be almost constant
            if(sz[u]>sz[v]){ // union by size
                p[v] = u; 
                sz[u]+=sz[v];
            }else{
                p[u] = v;
                sz[v] += sz[u];
            }
        }
    }
    int longestConsecutive(vector<int>& nums) {
        int n = nums.size();
        iota(p, p + n, 0);
        fill(sz, sz + n, 1);
        for(int i=0; i<n; i++){
            if(idx.find(nums[i])!=idx.end())
                continue;
            if(idx.find(nums[i]+1)!=idx.end())
                unite(i, idx[nums[i]+1]);
            if(idx.find(nums[i]-1)!=idx.end())
                unite(i, idx[nums[i]-1]);
            idx[nums[i]] = i;
        }
        return *max_element(sz, sz+n);
    }
};
```