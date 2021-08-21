# [239. Sliding Window Maximum (Hard)](https://leetcode.com/problems/sliding-window-maximum/)

<p>You are given an array of integers&nbsp;<code>nums</code>, there is a sliding window of size <code>k</code> which is moving from the very left of the array to the very right. You can only see the <code>k</code> numbers in the window. Each time the sliding window moves right by one position.</p>

<p>Return <em>the max sliding window</em>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> nums = [1,3,-1,-3,5,3,6,7], k = 3
<strong>Output:</strong> [3,3,5,5,6,7]
<strong>Explanation:</strong> 
Window position                Max
---------------               -----
[1  3  -1] -3  5  3  6  7       <strong>3</strong>
 1 [3  -1  -3] 5  3  6  7       <strong>3</strong>
 1  3 [-1  -3  5] 3  6  7      <strong> 5</strong>
 1  3  -1 [-3  5  3] 6  7       <strong>5</strong>
 1  3  -1  -3 [5  3  6] 7       <strong>6</strong>
 1  3  -1  -3  5 [3  6  7]      <strong>7</strong>
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> nums = [1], k = 1
<strong>Output:</strong> [1]
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> nums = [1,-1], k = 1
<strong>Output:</strong> [1,-1]
</pre>

<p><strong>Example 4:</strong></p>

<pre><strong>Input:</strong> nums = [9,11], k = 2
<strong>Output:</strong> [11]
</pre>

<p><strong>Example 5:</strong></p>

<pre><strong>Input:</strong> nums = [4,-2], k = 2
<strong>Output:</strong> [4]
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>4</sup> &lt;= nums[i] &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= k &lt;= nums.length</code></li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Queue](https://leetcode.com/tag/queue/), [Sliding Window](https://leetcode.com/tag/sliding-window/), [Heap (Priority Queue)](https://leetcode.com/tag/heap-priority-queue/), [Monotonic Queue](https://leetcode.com/tag/monotonic-queue/)

**Similar Questions**:
* [Minimum Window Substring (Hard)](https://leetcode.com/problems/minimum-window-substring/)
* [Min Stack (Easy)](https://leetcode.com/problems/min-stack/)
* [Longest Substring with At Most Two Distinct Characters (Medium)](https://leetcode.com/problems/longest-substring-with-at-most-two-distinct-characters/)
* [Paint House II (Hard)](https://leetcode.com/problems/paint-house-ii/)
* [Jump Game VI (Medium)](https://leetcode.com/problems/jump-game-vi/)

## Solution 1. Sliding Window using set

```cpp
// OJ: https://leetcode.com/problems/sliding-window-maximum/
// Author: A M A N
// Time : O(NlogN)
// Space: O(K)
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        vector<int> ans;
        int i=0, j=0;
        multiset<int> sett;
        while(j<nums.size()){
            int window = j-i+1;
            sett.insert(nums[j]);
            if(window<k){
                j++;
            }else if(window==k){
                ans.push_back(*sett.rbegin());
                sett.erase(sett.find(nums[i])); // erasing by reference deletes only that specific instance while erasing by value deletes all the instances with same value.
                i++, j++;
            }
        }
        return ans;
    }
};
```

using monotonically decreasing queue/list to erase the first element in O(1) time.

```cpp
// OJ: https://leetcode.com/problems/sliding-window-maximum/
// Author: A M A N
// Time : O(N)
// Space: O(K)
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        vector<int> ans;
        list<int> list; // maintain a monotonically decreasing list
        int i=0, j=0;
        while(j<nums.size()){
            while(!list.empty() and list.back()<nums[j])
                list.pop_back();
            list.push_back(nums[j]);
            int window = j-i+1;
            if(window<k){
                j++; // increase the size of window
            }else if(window==k){
                ans.push_back(list.front());
                if(list.front()==nums[i]) // first element of the window will be at the front OR might have been removed by an afterwards greater element 
                    list.pop_front();
                i++, j++; // slide the window
            }
        }
        return ans;
    }
};
```

## Solution 2. Using Max heap

```cpp
// OJ: https://leetcode.com/problems/sliding-window-maximum/
// Author: A M A N
// Time : O(NKlogK)
// Space: O(K)
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        vector<int> ans;
        priority_queue<pair<int, int>> pq; 
        int i=0, j=0;
        while(j<nums.size()){
            pq.push({nums[j], j});
            int window = j-i+1;
            if(window<k)
                j++;
            else if(window==k){
                while(pq.size() and pq.top().second < i) pq.pop();
                ans.push_back(pq.top().first);
                i++, j++;
            }
        }
        return ans;
    }
};
```