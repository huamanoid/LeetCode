# [253. Meeting Rooms II (Medium)](https://leetcode.com/problems/meeting-rooms-ii/)

<p>Given an array of meeting time intervals <code>intervals</code> where <code>intervals[i] = [start<sub>i</sub>, end<sub>i</sub>]</code>, return <em>the minimum number of conference rooms required</em>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<pre><strong>Input:</strong> intervals = [[0,30],[5,10],[15,20]]
<strong>Output:</strong> 2
</pre><p><strong>Example 2:</strong></p>
<pre><strong>Input:</strong> intervals = [[7,10],[2,4]]
<strong>Output:</strong> 1
</pre>
<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;=&nbsp;intervals.length &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= start<sub>i</sub> &lt; end<sub>i</sub> &lt;= 10<sup>6</sup></code></li>
</ul>


**Companies**:  
[Amazon](https://leetcode.com/company/amazon), [Bloomberg](https://leetcode.com/company/bloomberg), [Microsoft](https://leetcode.com/company/microsoft), [Google](https://leetcode.com/company/google), [Facebook](https://leetcode.com/company/facebook), [ByteDance](https://leetcode.com/company/bytedance), [Walmart Labs](https://leetcode.com/company/walmart-labs), [Adobe](https://leetcode.com/company/adobe), [eBay](https://leetcode.com/company/ebay), [Uber](https://leetcode.com/company/uber), [Qualtrics](https://leetcode.com/company/qualtrics), [Snapchat](https://leetcode.com/company/snapchat), [Apple](https://leetcode.com/company/apple), [Visa](https://leetcode.com/company/visa), [Goldman Sachs](https://leetcode.com/company/goldman-sachs), [Yandex](https://leetcode.com/company/yandex), [Docusign](https://leetcode.com/company/docusign)

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Two Pointers](https://leetcode.com/tag/two-pointers/), [Greedy](https://leetcode.com/tag/greedy/), [Sorting](https://leetcode.com/tag/sorting/), [Heap (Priority Queue)](https://leetcode.com/tag/heap-priority-queue/)

**Similar Questions**:
* [Merge Intervals (Medium)](https://leetcode.com/problems/merge-intervals/)
* [Meeting Rooms (Easy)](https://leetcode.com/problems/meeting-rooms/)
* [Minimum Number of Arrows to Burst Balloons (Medium)](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/)
* [Car Pooling (Medium)](https://leetcode.com/problems/car-pooling/)

## Solution 1. Sorting

Draw the intervals on X-axis to get better intuition

```cpp
// OJ: https://leetcode.com/problems/meeting-rooms-ii/
// Author: A M A N
// Time : O(NlogN)
// Space: O(N)
class Solution {
public:
    int minMeetingRooms(vector<vector<int>>& intervals) {
        map<int, int> mp; // key => timestamp, value => no of rooms open/close at that time
        for(auto v: intervals){
            mp[v[0]]++; // room occupied
            mp[v[1]]--; // room vacated
        }
        int noOfActiveRooms = 0, maxRooms=0;
        for(auto [a, v]: mp){
            noOfActiveRooms += v;
            maxRooms = max(maxRooms, noOfActiveRooms);
        }
        return maxRooms;
    }
};
```

## Solution 2. Min Heap

```cpp
// OJ: https://leetcode.com/problems/meeting-rooms-ii/
// Author: A M A N
// Time : O(NlogN)
// Space: O(N)
class Solution {
public:
    int minMeetingRooms(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end()); // sort the intervals wrt the start time
        priority_queue<int, vector<int>, greater<int>> pq; //min heap to track meeting end times
        //The size of the heap tells us the number of meetings going on at present.
        for(auto v: intervals){
            int start = v[0], end = v[1];
            // if any (one with smallest end time) previous room has been vacated before the start of new meeting
            if(!pq.empty() and pq.top() <= start) // then we use the same room 
                pq.pop();
            pq.push(end); // pushing the time when the room will be vacated
        }
        return pq.size(); // no of active rooms
    }
};
```

