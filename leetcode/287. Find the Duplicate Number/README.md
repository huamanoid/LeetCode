# [287. Find the Duplicate Number (Medium)](https://leetcode.com/problems/find-the-duplicate-number/)

<p>Given an array of integers <code>nums</code> containing&nbsp;<code>n + 1</code> integers where each integer is in the range <code>[1, n]</code> inclusive.</p>

<p>There is only <strong>one repeated number</strong> in <code>nums</code>, return <em>this&nbsp;repeated&nbsp;number</em>.</p>

<p>You must solve the problem <strong>without</strong> modifying the array <code>nums</code>&nbsp;and uses only constant extra space.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<pre><strong>Input:</strong> nums = [1,3,4,2,2]
<strong>Output:</strong> 2
</pre><p><strong>Example 2:</strong></p>
<pre><strong>Input:</strong> nums = [3,1,3,4,2]
<strong>Output:</strong> 3
</pre><p><strong>Example 3:</strong></p>
<pre><strong>Input:</strong> nums = [1,1]
<strong>Output:</strong> 1
</pre><p><strong>Example 4:</strong></p>
<pre><strong>Input:</strong> nums = [1,1,2]
<strong>Output:</strong> 1
</pre>
<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>nums.length == n + 1</code></li>
	<li><code>1 &lt;= nums[i] &lt;= n</code></li>
	<li>All the integers in <code>nums</code> appear only <strong>once</strong> except for <strong>precisely one integer</strong> which appears <strong>two or more</strong> times.</li>
</ul>

<p>&nbsp;</p>
<p><b>Follow up:</b></p>

<ul>
	<li>How can we prove that at least one duplicate number must exist in <code>nums</code>?</li>
	<li>Can you solve the problem in linear runtime complexity?</li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Two Pointers](https://leetcode.com/tag/two-pointers/), [Binary Search](https://leetcode.com/tag/binary-search/), [Bit Manipulation](https://leetcode.com/tag/bit-manipulation/)

**Similar Questions**:
* [First Missing Positive (Hard)](https://leetcode.com/problems/first-missing-positive/)
* [Single Number (Easy)](https://leetcode.com/problems/single-number/)
* [Linked List Cycle II (Medium)](https://leetcode.com/problems/linked-list-cycle-ii/)
* [Missing Number (Easy)](https://leetcode.com/problems/missing-number/)
* [Set Mismatch (Easy)](https://leetcode.com/problems/set-mismatch/)

## Solution 1. Using Indices as visited array

though it modifies the array but the modification can be reversed at the end by 
abs(nums[i]) for each i in nums;
```cpp
// OJ: https://leetcode.com/problems/find-the-duplicate-number/
// Author: A M A N
// Time : O(N)
// Space: O(1)
class Solution {
public:
    int findDuplicate(vector<int>& arr) {
        for(int i=0; i<arr.size(); i++){
            int k = abs(arr[i]); // handling negative indices
            if(arr[k-1] < 0) return k; // the number k has already been visited
            arr[k-1] *= -1;  // using indices as visited array 
        }
        return -1;
    }
};
```

## Solution 2. Floyd's Tortoise and Hare Algorithm

```
If there is `no duplicate` in the array, we can map each indexes to each numbers in this array. 
a mapping function `f(index) = number`

nums = [2,1,3], then the mapping function is 0->2, 1->1, 2->3
(Because index=3 exceeds the array's size, the sequence terminates!)

However, if there is `duplicate` in the array, the mapping function is `many-to-one`.

nums = [2,1,3,1], then the mapping function is 0->2, {1,3}->1, 2->3. Then the sequence we get `definitely has a cycle`. 
0->2->3->1->1->1->1->1->........ The starting point of this cycle is the duplicate number.

```

```

According to Floyd's algorithm, first step, if a cycle does exist, and you advance the `tortoise` one node each unit of time but the `hare` two nodes each unit of time, then `they will eventually meet`. 
`This is what the first while loop does.` The first while loop finds their meeting point

Now, take tortoise or hare to the start point of the list and keep the other one staying at the meeting point. Now, advance both of the animals one node each unit of time, the meeting point is the starting point of the cycle. This is what the second while loop does. The second while loop finds their meeting point.

`Proof` of second while loop:

Distance traveled by tortoise while meeting = `x + y`

Distance traveled by hare while meeting = `(x + y + z) + y = x + 2y + z`

Since hare travels with double the speed of tortoise,

so 2(x+y)= x+2y+z => x+2y+z = 2x+2y => `x=z`
```

```cpp
// OJ: https://leetcode.com/problems/find-the-duplicate-number/
// Author: A M A N
// Time : O(N)
// Space: O(1)
class Solution {
public:
    int findDuplicate(vector<int>& arr) {
        int slow = arr[0];
        int fast = arr[arr[0]];
        while(slow!=fast){ // Floyd says both the tortoise and hare would eventually `meet` if there's a cycle, 
            slow = arr[slow];
            fast = arr[arr[fast]];
        }//it stops at their meeting point
        fast = 0;
        while(slow!=fast){ // distance(meeting point, beginning of loop) = distance (starting point, beginning of loop)
            slow = arr[slow];
            fast = arr[fast];
        }
        return fast;
    }
};
```