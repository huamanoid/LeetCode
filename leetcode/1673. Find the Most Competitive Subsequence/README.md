# [1673. Find the Most Competitive Subsequence (Medium)](https://leetcode.com/problems/find-the-most-competitive-subsequence/)

<p>Given an integer array <code>nums</code> and a positive integer <code>k</code>, return <em>the most<strong> competitive</strong> subsequence of </em><code>nums</code> <em>of size </em><code>k</code>.</p>

<p>An array's subsequence is a resulting sequence obtained by erasing some (possibly zero) elements from the array.</p>

<p>We define that a subsequence <code>a</code> is more <strong>competitive</strong> than a subsequence <code>b</code> (of the same length) if in the first position where <code>a</code> and <code>b</code> differ, subsequence <code>a</code> has a number <strong>less</strong> than the corresponding number in <code>b</code>. For example, <code>[1,3,4]</code> is more competitive than <code>[1,3,5]</code> because the first position they differ is at the final number, and <code>4</code> is less than <code>5</code>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> nums = [3,5,2,6], k = 2
<strong>Output:</strong> [2,6]
<strong>Explanation:</strong> Among the set of every possible subsequence: {[3,5], [3,2], [3,6], [5,2], [5,6], [2,6]}, [2,6] is the most competitive.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> nums = [2,4,3,3,5,4,9,6], k = 4
<strong>Output:</strong> [2,3,3,4]
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= k &lt;= nums.length</code></li>
</ul>


**Companies**:  
[Amazon](https://leetcode.com/company/amazon)

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Stack](https://leetcode.com/tag/stack/), [Greedy](https://leetcode.com/tag/greedy/), [Monotonic Stack](https://leetcode.com/tag/monotonic-stack/)

**Similar Questions**:
* [Remove K Digits (Medium)](https://leetcode.com/problems/remove-k-digits/)
* [Smallest Subsequence of Distinct Characters (Medium)](https://leetcode.com/problems/smallest-subsequence-of-distinct-characters/)

## Solution 1. Monotonic Stack

```cpp
// OJ: https://leetcode.com/problems/find-the-most-competitive-subsequence/
// Author: A M A N
// Time : O(N)
// Space: O(k)
class Solution {
public:
    vector<int> mostCompetitive(vector<int>& nums, int k) {
        int n = nums.size();
        stack<int> st; //maintain a monotonic increasing stack
        for(int i=0; i<n; i++){
            // the last condition makes sure that there are enough elements after the current element
            // in the array to make a sequence of size k
            while(st.size() and st.top() > nums[i] and (n-1-i) + st.size()>=k) 
                st.pop();
            if(st.size()<k)st.push(nums[i]); 
        }
        vector<int> ans;
        while(!st.empty()){
            ans.push_back(st.top());
            st.pop();
        }
        reverse(ans.begin(), ans.end());
        return ans;
    }
};
```