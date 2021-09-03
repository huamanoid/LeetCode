# [238. Product of Array Except Self (Medium)](https://leetcode.com/problems/product-of-array-except-self/)

<p>Given an integer array <code>nums</code>, return <em>an array</em> <code>answer</code> <em>such that</em> <code>answer[i]</code> <em>is equal to the product of all the elements of</em> <code>nums</code> <em>except</em> <code>nums[i]</code>.</p>

<p>The product of any prefix or suffix of <code>nums</code> is <strong>guaranteed</strong> to fit in a <strong>32-bit</strong> integer.</p>

<p>You must write an algorithm that runs in&nbsp;<code>O(n)</code>&nbsp;time and without using the division operation.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<pre><strong>Input:</strong> nums = [1,2,3,4]
<strong>Output:</strong> [24,12,8,6]
</pre><p><strong>Example 2:</strong></p>
<pre><strong>Input:</strong> nums = [-1,1,0,-3,3]
<strong>Output:</strong> [0,0,9,0,0]
</pre>
<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-30 &lt;= nums[i] &lt;= 30</code></li>
	<li>The product of any prefix or suffix of <code>nums</code> is <strong>guaranteed</strong> to fit in a <strong>32-bit</strong> integer.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Follow up:</strong>&nbsp;Can you solve the problem in <code>O(1)&nbsp;</code>extra&nbsp;space complexity? (The output array <strong>does not</strong> count as extra space for space complexity analysis.)</p>


**Companies**:  
[Facebook](https://leetcode.com/company/facebook), [Amazon](https://leetcode.com/company/amazon), [Apple](https://leetcode.com/company/apple), [Asana](https://leetcode.com/company/asana), [Microsoft](https://leetcode.com/company/microsoft), [Adobe](https://leetcode.com/company/adobe), [Expedia](https://leetcode.com/company/expedia), [Nvidia](https://leetcode.com/company/nvidia), [Intel](https://leetcode.com/company/intel)

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Prefix Sum](https://leetcode.com/tag/prefix-sum/)

**Similar Questions**:
* [Trapping Rain Water (Hard)](https://leetcode.com/problems/trapping-rain-water/)
* [Maximum Product Subarray (Medium)](https://leetcode.com/problems/maximum-product-subarray/)
* [Paint House II (Hard)](https://leetcode.com/problems/paint-house-ii/)

## Solution 1. Prefix Sum

```cpp
// OJ: https://leetcode.com/problems/product-of-array-except-self/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        int n = nums.size();
        vector<int>left(n), right(n);
        left[0] = nums[0];
        for(int i=1; i<n; i++)
            left[i] = left[i-1]*nums[i];
        right[n-1] = nums[n-1];
        for(int i=n-2; i>=0; i--)
            right[i] = right[i+1]*nums[i];
        vector<int> ans;
        for(int i=0; i<n; i++){
            int p = (i?left[i-1]:1) * (i+1<n ? right[i+1]:1);
            ans.push_back(p);
        }
        return ans;
    }
};
```