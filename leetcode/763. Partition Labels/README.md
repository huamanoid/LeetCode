# [763. Partition Labels (Medium)](https://leetcode.com/problems/partition-labels/)

<p>You are given a string <code>s</code>. We want to partition the string into as many parts as possible so that each letter appears in at most one part.</p>

<p>Return <em>a list of integers representing the size of these parts</em>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> s = "ababcbacadefegdehijhklij"
<strong>Output:</strong> [9,7,8]
<strong>Explanation:</strong>
The partition is "ababcbaca", "defegde", "hijhklij".
This is a partition so that each letter appears in at most one part.
A partition like "ababcbacadefegde", "hijhklij" is incorrect, because it splits s into less parts.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> s = "eccbbbbdec"
<strong>Output:</strong> [10]
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 500</code></li>
	<li><code>s</code> consists of lowercase English letters.</li>
</ul>


**Companies**:  
[Amazon](https://leetcode.com/company/amazon), [Facebook](https://leetcode.com/company/facebook), [Apple](https://leetcode.com/company/apple)

**Related Topics**:  
[Hash Table](https://leetcode.com/tag/hash-table/), [Two Pointers](https://leetcode.com/tag/two-pointers/), [String](https://leetcode.com/tag/string/), [Greedy](https://leetcode.com/tag/greedy/)

**Similar Questions**:
* [Merge Intervals (Medium)](https://leetcode.com/problems/merge-intervals/)

## Solution 1. Greedy

```cpp
// OJ: https://leetcode.com/problems/partition-labels/
// Author: A M A N
// Time : O(N)
// Space: O(1)
class Solution {
public:
    vector<int> partitionLabels(string s) {
        unordered_map<char, int> last;
        for(int i=0; i<s.size(); i++)
            last[s[i]] = i;
        int prev = 0;
        vector<int> ans;
        unordered_set<char> vis;
        for(int i=0; i<s.size(); i++){
            if(last[s[i]]>i){
                vis.insert(s[i]);
            }else{
                bool canPartition = true;
                for(auto a: vis){
                    if(last[a]>i)
                        canPartition = false;
                }
                if(canPartition){
                    ans.push_back(i-prev+1);
                    prev = i+1;
                    vis.clear();
                }
            }
        }
        return ans;
    }
};
```

Shorter implementation using the following observation:
Once you include a char, the right bound of the partition must be greater than or equal to of the `last` idx of that char


```cpp
// OJ: https://leetcode.com/problems/partition-labels/
// Author: A M A N
// Time : O(N)
// Space: O(1)
class Solution {
public:
    vector<int> partitionLabels(string s) {
        unordered_map<char, int> last;
        for(int i=0; i<s.size(); i++)
            last[s[i]] = i;
        int rightBound = 0, prev = 0;
        vector<int> ans;
        for(int i=0; i<s.size(); i++){
            rightBound = max(rightBound, last[s[i]]);
            if(i==rightBound){
                ans.push_back(i-prev+1);
                prev = i+1;
            }
        }
        return ans;
    }
};
```