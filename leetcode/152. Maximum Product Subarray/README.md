# [152. Maximum Product Subarray (Medium)](https://leetcode.com/problems/maximum-product-subarray/)

<p>Given an integer array <code>nums</code>, find a contiguous non-empty subarray within the array that has the largest product, and return <em>the product</em>.</p>

<p>It is <strong>guaranteed</strong> that the answer will fit in a <strong>32-bit</strong> integer.</p>

<p>A <strong>subarray</strong> is a contiguous subsequence of the array.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> nums = [2,3,-2,4]
<strong>Output:</strong> 6
<strong>Explanation:</strong> [2,3] has the largest product 6.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> nums = [-2,0,-1]
<strong>Output:</strong> 0
<strong>Explanation:</strong> The result cannot be 2, because [-2,-1] is not a subarray.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>-10 &lt;= nums[i] &lt;= 10</code></li>
	<li>The product of any prefix or suffix of <code>nums</code> is <strong>guaranteed</strong> to fit in a <strong>32-bit</strong> integer.</li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/)

**Similar Questions**:
* [Maximum Subarray (Easy)](https://leetcode.com/problems/maximum-subarray/)
* [House Robber (Medium)](https://leetcode.com/problems/house-robber/)
* [Product of Array Except Self (Medium)](https://leetcode.com/problems/product-of-array-except-self/)
* [Maximum Product of Three Numbers (Easy)](https://leetcode.com/problems/maximum-product-of-three-numbers/)
* [Subarray Product Less Than K (Medium)](https://leetcode.com/problems/subarray-product-less-than-k/)

## Solution 1. DP (Recursive)

```cpp
// OJ: https://leetcode.com/problems/maximum-product-subarray/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    int ans = INT_MIN;
    set<pair<int, int>> dp; // visited 
    void solve(vector<int>&nums, int n, int temp){
        if(!n)return;        
        if(dp.count({n, temp})) // check if the the subtree has already been visited
            return;         
        dp.insert({n, temp});
        solve(nums, n-1, temp*nums[n-1]); //include
        solve(nums, n-1, nums[n-1]);      //exclude
        ans = max(ans, max(nums[n-1], temp*nums[n-1]));
    }
    int maxProduct(vector<int>& nums) {
        solve(nums, nums.size(), 1);
        return ans;
    }
};
```


## Solution 2. 

```cpp
// OJ: https://leetcode.com/problems/maximum-product-subarray/
// Author: A M A N
// Time : O(N)
// Space: O(1)
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int max_ = nums[0];
        int min_ = nums[0];
        int ans  = nums[0] ;
        
        for(int i=1; i<nums.size(); i++){
            if(nums[i]<0)
                swap(max_, min_);
            max_ = max(nums[i], max_*nums[i]);
            min_ = min(nums[i], min_*nums[i]);
            
            ans = max(ans, max_);
        }
        return ans;
    }   
};
```