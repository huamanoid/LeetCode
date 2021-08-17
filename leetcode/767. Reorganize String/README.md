# [767. Reorganize String (Medium)](https://leetcode.com/problems/reorganize-string/)

<p>Given a string <code>s</code>, rearrange the characters of <code>s</code> so that any two adjacent characters are not the same.</p>

<p>Return <em>any possible rearrangement of</em> <code>s</code> <em>or return</em> <code>""</code> <em>if not possible</em>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<pre><strong>Input:</strong> s = "aab"
<strong>Output:</strong> "aba"
</pre><p><strong>Example 2:</strong></p>
<pre><strong>Input:</strong> s = "aaab"
<strong>Output:</strong> ""
</pre>
<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 500</code></li>
	<li><code>s</code> consists of lowercase English letters.</li>
</ul>


**Companies**:  
[Amazon](https://leetcode.com/company/amazon), [Microsoft](https://leetcode.com/company/microsoft), [Wish](https://leetcode.com/company/wish), [Facebook](https://leetcode.com/company/facebook), [Google](https://leetcode.com/company/google), [Uber](https://leetcode.com/company/uber)

**Related Topics**:  
[Hash Table](https://leetcode.com/tag/hash-table/), [String](https://leetcode.com/tag/string/), [Greedy](https://leetcode.com/tag/greedy/), [Sorting](https://leetcode.com/tag/sorting/), [Heap (Priority Queue)](https://leetcode.com/tag/heap-priority-queue/), [Counting](https://leetcode.com/tag/counting/)

**Similar Questions**:
* [Rearrange String k Distance Apart (Hard)](https://leetcode.com/problems/rearrange-string-k-distance-apart/)
* [Task Scheduler (Medium)](https://leetcode.com/problems/task-scheduler/)

## Solution 1. Greedy using Heap

keep on substituting the characters with highest frequency, without making any two consecutive characters same.

```cpp
// OJ: https://leetcode.com/problems/reorganize-string/
// Author: A M A N
// Time : O(NlogN)
// Space: O(N)
class Solution {
public:
    string reorganizeString(string s) {
        unordered_map<char, int> mp;
        for(char c: s) mp[c]++;
        priority_queue<pair<int, int>> pq; // max heap for chars with max freq at top
        for(auto [a, v]: mp)
            pq.push({v, a});
        string ans;
        while(!pq.empty()){
            auto [f, c] = pq.top();
             if(ans.empty() or c!=ans[ans.size()-1]){ 
                ans+=c;
                f--; 
                pq.pop();
                if(f) pq.push({f, c});
            }else {
                pq.pop(); // pop the current char to find a char different from the ending character
                if(pq.empty())return ""; // if there's no char left other than the same char, return null
                auto [ff, cc] = pq.top(); pq.pop(); // second max char 
                ans+=cc;
                ff--; //decrement the count
                if(ff) pq.push({ff, cc});
                pq.push({f, c}); // push the original char which was similar to the ending char
            }
        }
        return ans;
    }
};
```