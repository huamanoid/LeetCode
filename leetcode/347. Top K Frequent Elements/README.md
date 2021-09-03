# [347. Top K Frequent Elements (Medium)](https://leetcode.com/problems/top-k-frequent-elements/)

<p>Given an integer array <code>nums</code> and an integer <code>k</code>, return <em>the</em> <code>k</code> <em>most frequent elements</em>. You may return the answer in <strong>any order</strong>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<pre><strong>Input:</strong> nums = [1,1,1,2,2,3], k = 2
<strong>Output:</strong> [1,2]
</pre><p><strong>Example 2:</strong></p>
<pre><strong>Input:</strong> nums = [1], k = 1
<strong>Output:</strong> [1]
</pre>
<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>k</code> is in the range <code>[1, the number of unique elements in the array]</code>.</li>
	<li>It is <strong>guaranteed</strong> that the answer is <strong>unique</strong>.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Follow up:</strong> Your algorithm's time complexity must be better than <code>O(n log n)</code>, where n is the array's size.</p>


**Companies**:  
[Facebook](https://leetcode.com/company/facebook), [Amazon](https://leetcode.com/company/amazon), [Microsoft](https://leetcode.com/company/microsoft), [Apple](https://leetcode.com/company/apple), [Yelp](https://leetcode.com/company/yelp), [Walmart Labs](https://leetcode.com/company/walmart-labs), [VMware](https://leetcode.com/company/vmware), [Bloomberg](https://leetcode.com/company/bloomberg), [Google](https://leetcode.com/company/google), [Uber](https://leetcode.com/company/uber), [Yahoo](https://leetcode.com/company/yahoo), [Adobe](https://leetcode.com/company/adobe), [eBay](https://leetcode.com/company/ebay), [HBO](https://leetcode.com/company/hbo)

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Hash Table](https://leetcode.com/tag/hash-table/), [Divide and Conquer](https://leetcode.com/tag/divide-and-conquer/), [Sorting](https://leetcode.com/tag/sorting/), [Heap (Priority Queue)](https://leetcode.com/tag/heap-priority-queue/), [Bucket Sort](https://leetcode.com/tag/bucket-sort/), [Counting](https://leetcode.com/tag/counting/), [Quickselect](https://leetcode.com/tag/quickselect/)

**Similar Questions**:
* [Word Frequency (Medium)](https://leetcode.com/problems/word-frequency/)
* [Kth Largest Element in an Array (Medium)](https://leetcode.com/problems/kth-largest-element-in-an-array/)
* [Sort Characters By Frequency (Medium)](https://leetcode.com/problems/sort-characters-by-frequency/)
* [Split Array into Consecutive Subsequences (Medium)](https://leetcode.com/problems/split-array-into-consecutive-subsequences/)
* [Top K Frequent Words (Medium)](https://leetcode.com/problems/top-k-frequent-words/)
* [K Closest Points to Origin (Medium)](https://leetcode.com/problems/k-closest-points-to-origin/)
* [Sort Features by Popularity (Medium)](https://leetcode.com/problems/sort-features-by-popularity/)

## Solution 1. Sorting

```cpp
// OJ: https://leetcode.com/problems/top-k-frequent-elements/
// Author: A M A N
// Time : O(NLogN)
// Space: O(N)
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int, int> freq;
        for(auto a: nums)
            freq[a]++;
        vector<pair<int, int>> v;
        for(auto [a,f]: freq)
            v.push_back({f, a});
        sort(v.begin(), v.end(), greater<pair<int, int>>());
        vector<int> ans;
        for(int i=0; i<v.size(); i++){
            if(ans.size()==k)break;
            ans.push_back(v[i].second);
        }
        return ans;
    }
};
```

## Solution 2. Map + MinHeap

```cpp
// OJ: https://leetcode.com/problems/top-k-frequent-elements/
// Author: A M A N
// Time : O(NlogK)
// Space: O(N)
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int, int> freq;
        for(auto a: nums) freq[a]++;
        // Min heap. default STL priority_queue is max heap
        priority_queue<int, vector<int>, greater<int>> pq;
        for(auto [a, f]: freq){
            pq.push(f);
            if(pq.size()>k)pq.pop();
        }
        vector<int> ans;
        int minFreq = pq.top(); // Kth smallest frequency
        for(auto [a, f]: freq){
            if(f>=minFreq)
                ans.push_back(a);
        }
        return ans;
    }
};
``