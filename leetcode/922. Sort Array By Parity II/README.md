# [922. Sort Array By Parity II (Easy)](https://leetcode.com/problems/sort-array-by-parity-ii/)

<p>Given an array of integers <code>nums</code>, half of the integers in <code>nums</code> are <strong>odd</strong>, and the other half are <strong>even</strong>.</p>

<p>Sort the array so that whenever <code>nums[i]</code> is odd, <code>i</code> is <strong>odd</strong>, and whenever <code>nums[i]</code> is even, <code>i</code> is <strong>even</strong>.</p>

<p>Return <em>any answer array that satisfies this condition</em>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> nums = [4,2,5,7]
<strong>Output:</strong> [4,5,2,7]
<strong>Explanation:</strong> [4,7,2,5], [2,5,4,7], [2,7,4,5] would also have been accepted.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> nums = [2,3]
<strong>Output:</strong> [2,3]
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>nums.length</code> is even.</li>
	<li>Half of the integers in <code>nums</code> are even.</li>
	<li><code>0 &lt;= nums[i] &lt;= 1000</code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Follow Up:</strong> Could you solve it in-place?</p>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Two Pointers](https://leetcode.com/tag/two-pointers/), [Sorting](https://leetcode.com/tag/sorting/)

## Solution 1.

```cpp
// OJ: https://leetcode.com/problems/sort-array-by-parity-ii/
// Author: A M A N
// Time : O(N^2)
// Space: O(1)
class Solution {
public:
    vector<int> sortArrayByParityII(vector<int>& nums) {
        for(int i=0; i<nums.size(); i++){
            if((nums[i]&1) == (i&1))continue;
            if(i&1){
                for(int j=i+1; j<nums.size(); j++){
                    if(nums[j]&1){
                        swap(nums[i], nums[j]);
                        break;
                    }
                }        
            }else{
                for(int j=i+1; j<nums.size(); j++){
                    if(nums[j]&1^1){
                        swap(nums[i], nums[j]);
                        break;
                    }
                }
            }
        }
        return nums;
    }
};
```