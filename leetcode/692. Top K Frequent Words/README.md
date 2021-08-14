# [692. Top K Frequent Words (Medium)](https://leetcode.com/problems/top-k-frequent-words/)

<p>Given an array of strings <code>words</code> and an integer <code>k</code>, return <em>the </em><code>k</code><em> most frequent strings</em>.</p>

<p>Return the answer <strong>sorted</strong> by <strong>the frequency</strong> from highest to lowest. Sort the words with the same frequency by their <strong>lexicographical order</strong>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> words = ["i","love","leetcode","i","love","coding"], k = 2
<strong>Output:</strong> ["i","love"]
<strong>Explanation:</strong> "i" and "love" are the two most frequent words.
Note that "i" comes before "love" due to a lower alphabetical order.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> words = ["the","day","is","sunny","the","the","the","sunny","is","is"], k = 4
<strong>Output:</strong> ["the","is","sunny","day"]
<strong>Explanation:</strong> "the", "is", "sunny" and "day" are the four most frequent words, with the number of occurrence being 4, 3, 2 and 1 respectively.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= words.length &lt;= 500</code></li>
	<li><code>1 &lt;= words[i] &lt;= 10</code></li>
	<li><code>words[i]</code> consists of lowercase English letters.</li>
	<li><code>k</code> is in the range <code>[1, The number of <strong>unique</strong> words[i]]</code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Follow-up:</strong> Could you solve it in <code>O(n log(k))</code> time and <code>O(n)</code> extra space?</p>


**Companies**:  
[Amazon](https://leetcode.com/company/amazon), [Microsoft](https://leetcode.com/company/microsoft), [Bloomberg](https://leetcode.com/company/bloomberg), [Google](https://leetcode.com/company/google), [Apple](https://leetcode.com/company/apple), [Salesforce](https://leetcode.com/company/salesforce), [Adobe](https://leetcode.com/company/adobe), [Oracle](https://leetcode.com/company/oracle), [Visa](https://leetcode.com/company/visa)

**Related Topics**:  
[Hash Table](https://leetcode.com/tag/hash-table/), [String](https://leetcode.com/tag/string/), [Trie](https://leetcode.com/tag/trie/), [Sorting](https://leetcode.com/tag/sorting/), [Heap (Priority Queue)](https://leetcode.com/tag/heap-priority-queue/), [Bucket Sort](https://leetcode.com/tag/bucket-sort/), [Counting](https://leetcode.com/tag/counting/)

**Similar Questions**:
* [Top K Frequent Elements (Medium)](https://leetcode.com/problems/top-k-frequent-elements/)
* [K Closest Points to Origin (Medium)](https://leetcode.com/problems/k-closest-points-to-origin/)
* [Sort Features by Popularity (Medium)](https://leetcode.com/problems/sort-features-by-popularity/)

## Solution 1. Sorting

```cpp
// OJ: https://leetcode.com/problems/top-k-frequent-words/
// Author: A M A N
// Time : O(NlogN)
// Space: O(N)
class Solution {
public:
    vector<string> topKFrequent(vector<string>& words, int k) {
        unordered_map<string, int> freq;
        for(auto s: words)
            freq[s]++;
        map<int, set<string>, greater<int>> mp; // map with non-increasing keys
        for(auto [s, f]: freq)
            mp[f].insert(s);
            
        vector<string> ans;
        for(auto [f, sett]: mp){
            for(auto s: sett){
                if(ans.size()==k)break;
                ans.push_back(s);
            }
        }
        return ans;
    }
};
```

## Solution 2. Heap

```cpp
// OJ: https://leetcode.com/problems/top-k-frequent-words/
// Author: A M A N
// Time : O(NlogK) logK because the size of the heap is constant
// Space: O(N)
class Solution {
    struct myComp{
      bool operator() (const pair<int, string>&a, const pair<int, string>&b){
          return (a.first==b.first ? a.second < b.second : a.first > b.first);
      }  
    };
public:
    vector<string> topKFrequent(vector<string>& words, int k) {
        unordered_map<string, int> freq;
        for(auto s: words)
            freq[s]++;
        priority_queue<pair<int, string>, vector<pair<int, string>>, myComp> pq;
        for(auto& [s, f]: freq){
            pq.push({f, s});
            if(pq.size()>k)pq.pop(); // maintaining the size K of max heap for log(K) operation time
        }
        vector<string> ans;
        while(pq.size()){
            ans.push_back(pq.top().second);
            pq.pop();
        }
        reverse(ans.begin(), ans.end()); // ?
        return ans;
    }
};
```