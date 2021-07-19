# [300. Longest Increasing Subsequence (Medium)](https://leetcode.com/problems/longest-increasing-subsequence/)

<p>Given an integer array <code>nums</code>, return the length of the longest strictly increasing subsequence.</p>

<p>A <strong>subsequence</strong> is a sequence that can be derived from an array by deleting some or no elements without changing the order of the remaining elements. For example, <code>[3,6,2,7]</code> is a subsequence of the array <code>[0,3,1,6,2,2,7]</code>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> nums = [10,9,2,5,3,7,101,18]
<strong>Output:</strong> 4
<strong>Explanation:</strong> The longest increasing subsequence is [2,3,7,101], therefore the length is 4.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> nums = [0,1,0,3,2,3]
<strong>Output:</strong> 4
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> nums = [7,7,7,7,7,7,7]
<strong>Output:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 2500</code></li>
	<li><code>-10<sup>4</sup> &lt;= nums[i] &lt;= 10<sup>4</sup></code></li>
</ul>

<p>&nbsp;</p>
<p><b>Follow up:</b>&nbsp;Can you come up with an algorithm that runs in&nbsp;<code>O(n log(n))</code> time complexity?</p>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Binary Search](https://leetcode.com/tag/binary-search/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/)

**Similar Questions**:
* [Increasing Triplet Subsequence (Medium)](https://leetcode.com/problems/increasing-triplet-subsequence/)
* [Russian Doll Envelopes (Hard)](https://leetcode.com/problems/russian-doll-envelopes/)
* [Maximum Length of Pair Chain (Medium)](https://leetcode.com/problems/maximum-length-of-pair-chain/)
* [Number of Longest Increasing Subsequence (Medium)](https://leetcode.com/problems/number-of-longest-increasing-subsequence/)
* [Minimum ASCII Delete Sum for Two Strings (Medium)](https://leetcode.com/problems/minimum-ascii-delete-sum-for-two-strings/)
* [Minimum Number of Removals to Make Mountain Array (Hard)](https://leetcode.com/problems/minimum-number-of-removals-to-make-mountain-array/)

## Solution 1. Recursion + Memoization

```cpp
// OJ: https://leetcode.com/problems/longest-increasing-subsequence/
// Author: A M A N
// Time : O(N*N*logN)
// Space: O(N)
class Solution {
public:
    
    map<pair<int, int>, int> dp;
    
    int LIS(vector<int>&nums, int idx, int prevIdx){
        if(idx==nums.size())return 0;
        
        if(prevIdx!=-1 and dp.find({idx, prevIdx})!=dp.end())
            return dp[{idx, prevIdx}];
        
        if(prevIdx == -1 or nums[idx] > nums[prevIdx])
            return dp[{idx, prevIdx}] = max(1+LIS(nums, idx+1, idx), LIS(nums, idx+1, prevIdx));
        else
            return  dp[{idx, prevIdx}] = LIS(nums, idx+1, prevIdx);
    }
    
    int lengthOfLIS(vector<int>& nums) {
        return LIS(nums, 0, -1);
    }
};
```

Using 2D array instead of map to save time.
```cpp
// OJ: https://leetcode.com/problems/longest-increasing-subsequence/
// Author: A M A N
// Time : O(N^2)
// Space: O(N)
class Solution {
public:
    // map<pair<int, int>, int> dp;
    int dp[2501][2501];
    int LIS(vector<int>&nums, int idx, int prevIdx){
        if(idx==nums.size())return 0;
        if(prevIdx!=-1 and dp[idx][prevIdx]!=-1 )
            return dp[idx][prevIdx];
        int a=0, b=0;
        if(prevIdx == -1 or nums[idx] > nums[prevIdx])
            a =  max(1+LIS(nums, idx+1, idx), LIS(nums, idx+1, prevIdx)); // max(pick, skip)
        else
            b = LIS(nums, idx+1, prevIdx); // skip
        if(prevIdx!=-1)
            return dp[idx][prevIdx] = max(a, b);
        else
            return max(a, b);
    }
    int lengthOfLIS(vector<int>& nums) {
        memset(dp, -1, sizeof dp);
        return LIS(nums, 0, -1);
    }
};
```

## Solution 2. DP

```cpp
// OJ: https://leetcode.com/problems/longest-increasing-subsequence/
// Author: A M A N
// Time : O(N*N)
// Space: O(N)
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        int n = nums.size();
        int dp[n];
        dp[0] = 1;
        for(int i=1; i<n; i++){
            dp[i] = 1;
            for(int j=0; j<i; j++){
                if(nums[i] > nums[j])
                    dp[i] = max(dp[i], 1+dp[j]);
            }
        }
        return *max_element(dp, dp+n);
    }
};
```


## Solution 3. Binary Search

> vector v having a length of the longest found increasing sub-sequence

> <b>So it doesn't contain that subsequence.</b> Only it's length is valid.

```
Dry Run:
[1,2,7,8,3,4,5,9,0]
1 -> [1]
2 -> [1,2]
7 -> [1,2,7]
8 -> [1,2,7,8]
3 -> [1,2,3,8]  // we replaced 7 with 3, since for the longest sequence we need only the last number and 1,2,3 is our new shorter sequence
4 -> [1,2,3,4] // we replaced 8 with 4, since the max len is the same but 4 has more chances for longer sequence
5 -> [1,2,3,4,5]
9 -> [1,2,3,4,5,9]
0 -> [0,2,3,4,5,9] // we replaced 1 with 0, so that it can become a new sequence

So in the end our res contains [0,2,3,4,5,9] which is not a found sequence, but it has the length of the valid answer = 6.
```

```cpp
// OJ: https://leetcode.com/problems/longest-increasing-subsequence/
// Author: github.com/lzl124631x
// Time: O(NlogN)
// Space: O(N)
class Solution {
public:
    int lengthOfLIS(vector<int>& A) {
        vector<int> v;
        for (int e : A) {
            auto i = lower_bound(begin(v), end(v), e);
            if (i == end(v)) v.push_back(e); 
            else *i = e; // to update the element in the vector v.
        }
        return v.size();
    }
};
```