# [75. Sort Colors (Medium)](https://leetcode.com/problems/sort-colors/)

<p>Given an array <code>nums</code> with <code>n</code> objects colored red, white, or blue, sort them <strong><a href="https://en.wikipedia.org/wiki/In-place_algorithm" target="_blank">in-place</a> </strong>so that objects of the same color are adjacent, with the colors in the order red, white, and blue.</p>

<p>We will use the integers <code>0</code>, <code>1</code>, and <code>2</code> to represent the color red, white, and blue, respectively.</p>

<p>You must solve this problem without using the library's sort function.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<pre><strong>Input:</strong> nums = [2,0,2,1,1,0]
<strong>Output:</strong> [0,0,1,1,2,2]
</pre><p><strong>Example 2:</strong></p>
<pre><strong>Input:</strong> nums = [2,0,1]
<strong>Output:</strong> [0,1,2]
</pre><p><strong>Example 3:</strong></p>
<pre><strong>Input:</strong> nums = [0]
<strong>Output:</strong> [0]
</pre><p><strong>Example 4:</strong></p>
<pre><strong>Input:</strong> nums = [1]
<strong>Output:</strong> [1]
</pre>
<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>n == nums.length</code></li>
	<li><code>1 &lt;= n &lt;= 300</code></li>
	<li><code>nums[i]</code> is <code>0</code>, <code>1</code>, or <code>2</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Follow up:</strong>&nbsp;Could you come up with a one-pass algorithm using only&nbsp;constant extra space?</p>


**Companies**:  
[Microsoft](https://leetcode.com/company/microsoft), [Amazon](https://leetcode.com/company/amazon), [Adobe](https://leetcode.com/company/adobe), [Facebook](https://leetcode.com/company/facebook), [Apple](https://leetcode.com/company/apple), [Nvidia](https://leetcode.com/company/nvidia), [Swiggy](https://leetcode.com/company/swiggy)

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Two Pointers](https://leetcode.com/tag/two-pointers/), [Sorting](https://leetcode.com/tag/sorting/)

**Similar Questions**:
* [Sort List (Medium)](https://leetcode.com/problems/sort-list/)
* [Wiggle Sort (Medium)](https://leetcode.com/problems/wiggle-sort/)
* [Wiggle Sort II (Medium)](https://leetcode.com/problems/wiggle-sort-ii/)

## Solution 1.

```cpp
// OJ: https://leetcode.com/problems/sort-colors/
// Author: A M A N
// Time : O(N)
// Space: O(1)
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int a = 0, b = 0, c = 0;
        for(auto &n: nums) 
            if(n==0) a++;
            else if(n==1) b++;
            else c++;
        for(int i=0; i<a; i++)       nums[i]=0;
        for(int i=a; i<a+b; i++)     nums[i]=1;
        for(int i=a+b; i<a+b+c; i++) nums[i]=2;
    }
};
```

## Solution 2. 

```cpp
// OJ: https://leetcode.com/problems/sort-colors/
// Author: A M A N
// Time : O(N)
// Space: O(1)
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int p0 = 0, p1 = 0, p2 = 0;
        for(int n: nums){
            if(n==0){ // to add 0, the bigger numbers (1 and 2) must be shifted
                nums[p2++] = 2;
                nums[p1++] = 1;
                nums[p0++] = 0;
            }else if(n==1){ 
                nums[p2++] = 2;
                nums[p1++] = 1;
            }else 
                nums[p2++] = 2;
        }
    }
};
```