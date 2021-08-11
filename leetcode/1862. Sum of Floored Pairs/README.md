# [1862. Sum of Floored Pairs (Hard)](https://leetcode.com/problems/sum-of-floored-pairs/)

<p>Given an integer array <code>nums</code>, return the sum of <code>floor(nums[i] / nums[j])</code> for all pairs of indices <code>0 &lt;= i, j &lt; nums.length</code> in the array. Since the answer may be too large, return it <strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>The <code>floor()</code> function returns the integer part of the division.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> nums = [2,5,9]
<strong>Output:</strong> 10
<strong>Explanation:</strong>
floor(2 / 5) = floor(2 / 9) = floor(5 / 9) = 0
floor(2 / 2) = floor(5 / 5) = floor(9 / 9) = 1
floor(5 / 2) = 2
floor(9 / 2) = 4
floor(9 / 5) = 1
We calculate the floor of the division for every pair of indices in the array then sum them up.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> nums = [7,7,7,7,7,7,7]
<strong>Output:</strong> 49
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Math](https://leetcode.com/tag/math/), [Binary Search](https://leetcode.com/tag/binary-search/), [Prefix Sum](https://leetcode.com/tag/prefix-sum/)

## Solution 1. Binary Search

```cpp
// OJ: https://leetcode.com/problems/sum-of-floored-pairs/
// Author: A M A N
// Time : O(N*K*logN) where K is the ratio of biggest and smallest number
// Space: O(1)
class Solution {
public:
    const int M = 1e9+7;
    int sumOfFlooredPairs(vector<int>& nums) {
        int n = nums.size();
        sort(nums.begin(), nums.end());
        int mx = nums[n-1];
        int ans = 0;
        for(int i=n-1; i>=0; i--){
            int times = 1;
            while(i and nums[i]==nums[i-1]){
                i--;
                times++;
            }
            int a = nums[i], prev = i;
            for(int k=2; k <= (mx+a-1)/a+1; k++){
                int next = a*k;
                int nextIdx = lower_bound(nums.begin()+i, nums.end(), next) - nums.begin();
                long long score = (k-1)*(nextIdx - prev);
                (ans += (1LL*times*score)%M)%=M;
                prev = nextIdx;
            }
        }
        return ans;
    }
};
```