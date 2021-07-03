# [681. Next Closest Time (Medium)](https://leetcode.com/problems/next-closest-time/)

<p>Given a <code>time</code> represented in the format <code>"HH:MM"</code>, form the next closest time by reusing the current digits. There is no limit on how many times a digit can be reused.</p>

<p>You may assume the given input string is always valid. For example, <code>"01:34"</code>, <code>"12:09"</code> are all valid. <code>"1:34"</code>, <code>"12:9"</code> are all invalid.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> time = "19:34"
<strong>Output:</strong> "19:39"
<strong>Explanation:</strong> The next closest time choosing from digits <strong>1</strong>, <strong>9</strong>, <strong>3</strong>, <strong>4</strong>, is <strong>19:39</strong>, which occurs 5 minutes later.
It is not <strong>19:33</strong>, because this occurs 23 hours and 59 minutes later.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> time = "23:59"
<strong>Output:</strong> "22:22"
<strong>Explanation:</strong> The next closest time choosing from digits <strong>2</strong>, <strong>3</strong>, <strong>5</strong>, <strong>9</strong>, is <strong>22:22</strong>.
It may be assumed that the returned time is next day's time since it is smaller than the input time numerically.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>time.length == 5</code></li>
	<li><code>time</code> is a valid time in the form <code>"HH:MM"</code>.</li>
	<li><code>0 &lt;= HH &lt; 24</code></li>
	<li><code>0 &lt;= MM &lt; 60</code></li>
</ul>


**Companies**:  
[Facebook](https://leetcode.com/company/facebook)

**Related Topics**:  
[String](https://leetcode.com/tag/string/), [Enumeration](https://leetcode.com/tag/enumeration/)

## Solution 1. Brute-force

Generate all the possible permuations (constant) and
find the optimal.

```cpp
// OJ: https://leetcode.com/problems/next-closest-time/
// Author: A M A N
// Time : O(1)
// Space: O(1)
};
class Solution {
public:
    bool isValid(string time){
        int H = stoi(time.substr(0, 2));
        int M = stoi(time.substr(3));
        if(H>=24) return false;
        if(M>=60) return false;
        return true;
    }
    int difference(string t1, string t2){
        int H1 = stoi(t1.substr(0, 2)), H2 = stoi(t2.substr(0, 2));
        int M1 = stoi(t1.substr(3)), M2 = stoi(t2.substr(3));
        int time1 = H1*60 + M1,  time2 = H2*60 + M2;
        if(time1<=time2)
            return time2-time1;
        return difference(t1, "23:59") + time2;        
    }
    vector<string> perm;
    void generatePermutation(vector<char>v, string time, int idx, string temp){
        if(idx>=time.size()){
            perm.push_back(temp);
            return;
        }
        if(idx!=2) // ":"
            for(auto c: v){
                temp[idx] = c;
                generatePermutation(v, time, idx+1, temp);
            }
        else
            generatePermutation(v, time, idx+1, temp);
    }
    string nextClosestTime(string time) {
        vector<char> v {time[0], time[1], time[3], time[4]};
        generatePermutation(v, time, 0, time);
        int cnt = INT_MAX;
        string ans = "-1";
        for(auto p: perm)
            if(isValid(p) and p!=time and difference(time, p)<cnt){
                cnt = difference(time, p);
                ans = p;
            }
        return ans=="-1"?time:ans;
    }
};
```