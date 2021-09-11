# [729. My Calendar I (Medium)](https://leetcode.com/problems/my-calendar-i/)

<p>You are implementing a program to use as your calendar. We can add a new event if adding the event will not cause a <strong>double booking</strong>.</p>

<p>A <strong>double booking</strong> happens when two events have some non-empty intersection (i.e., some moment is common to both events.).</p>

<p>The event can be represented as a pair of integers <code>start</code> and <code>end</code> that represents a booking on the half-open interval <code>[start, end)</code>, the range of real numbers <code>x</code> such that <code>start &lt;= x &lt; end</code>.</p>

<p>Implement the <code>MyCalendar</code> class:</p>

<ul>
	<li><code>MyCalendar()</code> Initializes the calendar object.</li>
	<li><code>boolean book(int start, int end)</code> Returns <code>true</code> if the event can be added to the calendar successfully without causing a <strong>double booking</strong>. Otherwise, return <code>false</code> and do not add the event to the calendar.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input</strong>
["MyCalendar", "book", "book", "book"]
[[], [10, 20], [15, 25], [20, 30]]
<strong>Output</strong>
[null, true, false, true]

<strong>Explanation</strong>
MyCalendar myCalendar = new MyCalendar();
myCalendar.book(10, 20); // return True
myCalendar.book(15, 25); // return False, It can not be booked because time 15 is already booked by another event.
myCalendar.book(20, 30); // return True, The event can be booked, as the first event takes every time less than 20, but not including 20.</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>0 &lt;= start &lt; end &lt;= 10<sup>9</sup></code></li>
	<li>At most <code>1000</code> calls will be made to <code>book</code>.</li>
</ul>


**Companies**:  
[Google](https://leetcode.com/company/google), [eBay](https://leetcode.com/company/ebay), [Facebook](https://leetcode.com/company/facebook)

**Related Topics**:  
[Design](https://leetcode.com/tag/design/), [Segment Tree](https://leetcode.com/tag/segment-tree/), [Ordered Set](https://leetcode.com/tag/ordered-set/)

**Similar Questions**:
* [My Calendar II (Medium)](https://leetcode.com/problems/my-calendar-ii/)
* [My Calendar III (Hard)](https://leetcode.com/problems/my-calendar-iii/)

## Solution 1. Naive

```cpp
// OJ: https://leetcode.com/problems/my-calendar-i/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class MyCalendar {
public:
    vector<pair<int, int>> books;
    MyCalendar() {
        books.clear();
    }
    bool book(int start, int end) {
        for(auto &p : books){
            int L = p.first, R = p.second;
            if(end>L and end<=R) return false;
            if(start>=L and start<R) return false;
            if(start<=L and end>=R) return false;
        }
        books.push_back({start, end});
        return true;
    }
};
/**
 * Your MyCalendar object will be instantiated and called as such:
 * MyCalendar* obj = new MyCalendar();
 * bool param_1 = obj->book(start,end);
 */
```

## Solution 2. Sorting

```cpp

// OJ: https://leetcode.com/problems/my-calendar-i/
// Author: A M A N
// Time : O(logN)
// Space: O(N)
class MyCalendar {
public:
    set<pair<int, int>> books;
    MyCalendar() {
        books.clear();
    }
    bool book(int start, int end) {
        // first element with key not go before k (i.e., either it is equivalent or goes after).
        auto next = books.lower_bound({start, end});
        if(next!=books.end() and next->first<end) return false;
        if(next!=books.begin() and start < (--next)->second) return false;
        books.insert({start, end});
        return true;
    }
};
/**
 * Your MyCalendar object will be instantiated and called as such:
 * MyCalendar* obj = new MyCalendar();
 * bool param_1 = obj->book(start,end);
 */
```