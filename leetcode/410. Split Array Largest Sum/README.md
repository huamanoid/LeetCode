# [410. Split Array Largest Sum (Hard)](https://leetcode.com/problems/split-array-largest-sum/)

<p>Given an array <code>nums</code> which consists of non-negative integers and an integer <code>m</code>, you can split the array into <code>m</code> non-empty continuous subarrays.</p>

<p>Write an algorithm to minimize the largest sum among these <code>m</code> subarrays.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> nums = [7,2,5,10,8], m = 2
<strong>Output:</strong> 18
<strong>Explanation:</strong>
There are four ways to split nums into two subarrays.
The best way is to split it into [7,2,5] and [10,8],
where the largest sum among the two subarrays is only 18.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> nums = [1,2,3,4,5], m = 2
<strong>Output:</strong> 9
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> nums = [1,4,4], m = 3
<strong>Output:</strong> 4
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>6</sup></code></li>
	<li><code>1 &lt;= m &lt;= min(50, nums.length)</code></li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Binary Search](https://leetcode.com/tag/binary-search/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/), [Greedy](https://leetcode.com/tag/greedy/)

## Solution 1. Binary Search on answers

```cpp
// OJ: https://leetcode.com/problems/split-array-largest-sum/
// Author: A M A N
// Time : O(N*log(S)) where S is the sum of all elements in nums
// Space: O(1)
class Solution {
public:
    bool isValid(vector<int>&nums, int maxSplits, int maxSum){
        int sum = 0, splits=1; 
        for(int i=0; i<nums.size(); i++){
            if(nums[i]>maxSum)
                return false;
            sum += nums[i];
            if(sum>maxSum){
                sum = nums[i];
                splits++;
            }
        }
        return splits<=maxSplits;
    }
    int splitArray(vector<int>& nums, int m) {
        int ans = -1;
        int L = 0;
        int R = accumulate(nums.begin(), nums.end(), 0);
        while(L<=R){
            int M = L + (R-L)/2;
            if(isValid(nums, m, M)){
                ans = M;
                R = M-1;
            }else
                L = M+1;
        }
        return ans;
    }
};
```