# [56. Merge Intervals (Medium)](https://leetcode.com/problems/merge-intervals/)

<p>Given an array&nbsp;of <code>intervals</code>&nbsp;where <code>intervals[i] = [start<sub>i</sub>, end<sub>i</sub>]</code>, merge all overlapping intervals, and return <em>an array of the non-overlapping intervals that cover all the intervals in the input</em>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> intervals = [[1,3],[2,6],[8,10],[15,18]]
<strong>Output:</strong> [[1,6],[8,10],[15,18]]
<strong>Explanation:</strong> Since intervals [1,3] and [2,6] overlaps, merge them into [1,6].
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> intervals = [[1,4],[4,5]]
<strong>Output:</strong> [[1,5]]
<strong>Explanation:</strong> Intervals [1,4] and [4,5] are considered overlapping.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= intervals.length &lt;= 10<sup>4</sup></code></li>
	<li><code>intervals[i].length == 2</code></li>
	<li><code>0 &lt;= start<sub>i</sub> &lt;= end<sub>i</sub> &lt;= 10<sup>4</sup></code></li>
</ul>


**Companies**:  
[JPMorgan](https://leetcode.com/company/jpmorgan), [Facebook](https://leetcode.com/company/facebook), [Amazon](https://leetcode.com/company/amazon), [Microsoft](https://leetcode.com/company/microsoft), [Google](https://leetcode.com/company/google), [Bloomberg](https://leetcode.com/company/bloomberg), [Adobe](https://leetcode.com/company/adobe), [Apple](https://leetcode.com/company/apple), [Salesforce](https://leetcode.com/company/salesforce), [VMware](https://leetcode.com/company/vmware), [Uber](https://leetcode.com/company/uber), [eBay](https://leetcode.com/company/ebay), [Cisco](https://leetcode.com/company/cisco), [ByteDance](https://leetcode.com/company/bytedance), [Paypal](https://leetcode.com/company/paypal), [Walmart Labs](https://leetcode.com/company/walmart-labs), [Oracle](https://leetcode.com/company/oracle), [Yandex](https://leetcode.com/company/yandex), [Wish](https://leetcode.com/company/wish), [Splunk](https://leetcode.com/company/splunk)

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Sorting](https://leetcode.com/tag/sorting/)

**Similar Questions**:
* [Insert Interval (Medium)](https://leetcode.com/problems/insert-interval/)
* [Meeting Rooms (Easy)](https://leetcode.com/problems/meeting-rooms/)
* [Meeting Rooms II (Medium)](https://leetcode.com/problems/meeting-rooms-ii/)
* [Teemo Attacking (Easy)](https://leetcode.com/problems/teemo-attacking/)
* [Add Bold Tag in String (Medium)](https://leetcode.com/problems/add-bold-tag-in-string/)
* [Range Module (Hard)](https://leetcode.com/problems/range-module/)
* [Employee Free Time (Hard)](https://leetcode.com/problems/employee-free-time/)
* [Partition Labels (Medium)](https://leetcode.com/problems/partition-labels/)
* [Interval List Intersections (Medium)](https://leetcode.com/problems/interval-list-intersections/)

## Solution 1. Sorting

```cpp
// OJ: https://leetcode.com/problems/merge-intervals/
// Author: A M A N
// Time : O(NlogN)
// Space: O(N)
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        vector<vector<int>> ans;
        int n = intervals.size();
        sort(intervals.begin(), intervals.end());
        for(int i=0; i<n; i++){
            int start = intervals[i][0], end = intervals[i][1];
            while(i+1<n and intervals[i+1][0]<=end){
                end = max(intervals[i+1][1], end);
                i++;
            }
            ans.push_back({start, end});
        }
        return ans;
    }
};
```