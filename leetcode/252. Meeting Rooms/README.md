# [252. Meeting Rooms (Easy)](https://leetcode.com/problems/meeting-rooms/)

<p>Given an array of meeting time <code>intervals</code>&nbsp;where <code>intervals[i] = [start<sub>i</sub>, end<sub>i</sub>]</code>, determine if a person could attend all meetings.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<pre><strong>Input:</strong> intervals = [[0,30],[5,10],[15,20]]
<strong>Output:</strong> false
</pre><p><strong>Example 2:</strong></p>
<pre><strong>Input:</strong> intervals = [[7,10],[2,4]]
<strong>Output:</strong> true
</pre>
<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>0 &lt;= intervals.length &lt;= 10<sup>4</sup></code></li>
	<li><code>intervals[i].length == 2</code></li>
	<li><code>0 &lt;= start<sub>i</sub> &lt;&nbsp;end<sub>i</sub> &lt;= 10<sup>6</sup></code></li>
</ul>


**Companies**:  
[Amazon](https://leetcode.com/company/amazon), [Facebook](https://leetcode.com/company/facebook), [Microsoft](https://leetcode.com/company/microsoft), [Bloomberg](https://leetcode.com/company/bloomberg), [Adobe](https://leetcode.com/company/adobe)

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Sorting](https://leetcode.com/tag/sorting/)

**Similar Questions**:
* [Merge Intervals (Medium)](https://leetcode.com/problems/merge-intervals/)
* [Meeting Rooms II (Medium)](https://leetcode.com/problems/meeting-rooms-ii/)

## Solution 1. Sorting

```cpp
// OJ: https://leetcode.com/problems/meeting-rooms/
// Author: A M A N
// Time : O(N)
// Space: O(1)
class Solution {
public:
    bool canAttendMeetings(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end());
        for(int i=0; i+1<intervals.size(); i++){
            if(intervals[i][1]>intervals[i+1][0])
                return false;
        }
        return true;
    }
};
```

## Solution 2.

Draw the intervals on X-axis to get better intuition

```cpp
// OJ: https://leetcode.com/problems/meeting-rooms/
// Author: A M A N
// Time : O(NlogN)
// Space: O(N)
class Solution {
public:
    bool canAttendMeetings(vector<vector<int>>& intervals) {
        map<int, int> mp; // key => timestamp, value => no of rooms open/close at that time
        for(auto v: intervals){
            mp[v[0]]++; // room occupied
            mp[v[1]]--; // room vacated
        }
        int noOfActiveRooms = 0, maxRooms=1;
        for(auto [a, v]: mp){
            noOfActiveRooms += v;
            maxRooms = max(maxRooms, noOfActiveRooms);
        }
        return maxRooms==1;
    }
};
```