# [1118. Number of Days in a Month (Easy)](https://leetcode.com/problems/number-of-days-in-a-month/)

<p>Given a year <code>year</code> and a month <code>month</code>, return <em>the number of days of that month</em>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<pre><strong>Input:</strong> year = 1992, month = 7
<strong>Output:</strong> 31
</pre><p><strong>Example 2:</strong></p>
<pre><strong>Input:</strong> year = 2000, month = 2
<strong>Output:</strong> 29
</pre><p><strong>Example 3:</strong></p>
<pre><strong>Input:</strong> year = 1900, month = 2
<strong>Output:</strong> 28
</pre>
<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1583 &lt;= year &lt;= 2100</code></li>
	<li><code>1 &lt;= month &lt;= 12</code></li>
</ul>


**Companies**:  
[Amazon](https://leetcode.com/company/amazon)

**Related Topics**:  
[Math](https://leetcode.com/tag/math/)

## Solution 1.

```cpp
// OJ: https://leetcode.com/problems/number-of-days-in-a-month/
// Author: A M A N
// Time : O(1)
// Space: O(1)
class Solution {
public:
    int numberOfDays(int year, int month) {
        month--;
        vector<int> days={31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};
        if(month==1)
            if(year%100==0){
                if(year%400==0)
                    return 29;
                else
                    return 28;
            }else if(year%4==0){
                return 29;
            }else return 28;
        return days[month];
    }
};
```