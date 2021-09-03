# [1760. Minimum Limit of Balls in a Bag (Medium)](https://leetcode.com/problems/minimum-limit-of-balls-in-a-bag/)

<p>You are given an integer array <code>nums</code> where the <code>i<sup>th</sup></code> bag contains <code>nums[i]</code> balls. You are also given an integer <code>maxOperations</code>.</p>

<p>You can perform the following operation at most <code>maxOperations</code> times:</p>

<ul>
	<li>Take any bag of balls and divide it into two new bags with a <strong>positive </strong>number of balls.

	<ul>
		<li>For example, a bag of <code>5</code> balls can become two new bags of <code>1</code> and <code>4</code> balls, or two new bags of <code>2</code> and <code>3</code> balls.</li>
	</ul>
	</li>
</ul>

<p>Your penalty is the <strong>maximum</strong> number of balls in a bag. You want to <strong>minimize</strong> your penalty after the operations.</p>

<p>Return <em>the minimum possible penalty&nbsp;after performing the operations</em>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> nums = [9], maxOperations = 2
<strong>Output:</strong> 3
<strong>Explanation:</strong> 
- Divide the bag with 9 balls into two bags of sizes 6 and 3. [<strong><u>9</u></strong>] -&gt; [6,3].
- Divide the bag with 6 balls into two bags of sizes 3 and 3. [<strong><u>6</u></strong>,3] -&gt; [3,3,3].
The bag with the most number of balls has 3 balls, so your penalty is 3 and you should return 3.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> nums = [2,4,8,2], maxOperations = 4
<strong>Output:</strong> 2
<strong>Explanation:</strong>
- Divide the bag with 8 balls into two bags of sizes 4 and 4. [2,4,<strong><u>8</u></strong>,2] -&gt; [2,4,4,4,2].
- Divide the bag with 4 balls into two bags of sizes 2 and 2. [2,<strong><u>4</u></strong>,4,4,2] -&gt; [2,2,2,4,4,2].
- Divide the bag with 4 balls into two bags of sizes 2 and 2. [2,2,2,<strong><u>4</u></strong>,4,2] -&gt; [2,2,2,2,2,4,2].
- Divide the bag with 4 balls into two bags of sizes 2 and 2. [2,2,2,2,2,<strong><u>4</u></strong>,2] -&gt; [2,2,2,2,2,2,2,2].
The bag with the most number of balls has 2 balls, so your penalty is 2 an you should return 2.
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> nums = [7,17], maxOperations = 2
<strong>Output:</strong> 7
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= maxOperations, nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Binary Search](https://leetcode.com/tag/binary-search/)

## Solution 1. Binary Search on answers

TLE
```cpp
// OJ: https://leetcode.com/problems/minimum-limit-of-balls-in-a-bag/
// Author: A M A N
// Time : O()
// Space: O(1)
class Solution {
public:
    bool isValid(vector<int>nums, int maxOperations, int mx){
        int cnt=0;
        for(int i=0; i<nums.size(); i++){
            if(nums[i]>mx){
                nums.push_back(nums[i]-mx); //brute force way 
                cnt++;
            }
        }
        return cnt<=maxOperations;
    }
    int minimumSize(vector<int>& nums, int maxOperations) {
        int L = 0, ans = -1;
        int R = *max_element(nums.begin(), nums.end());
        while(L<=R){
            int M = L + (R-L)/2;
            if(isValid(nums, maxOperations, M)){
                ans = M;
                R = M-1;
            }else
                L = M+1;
        }
        return ans;
    }
};
```



```cpp
// OJ: https://leetcode.com/problems/minimum-limit-of-balls-in-a-bag/
// Author: A M A N
// Time : O(N*log(1e9))
// Space: O(1)
class Solution {
public:
    bool isValid(vector<int>nums, int maxOperations, int mx){
        int cnt=0;
        for(int i=0; i<nums.size(); i++){
            cnt += (nums[i]-1)/mx; // the current num will have to be further broken into (nums[i]-1)/mx times, if it's less than mx then its just 0;
        }
        return cnt<=maxOperations;
    }
    int minimumSize(vector<int>& nums, int maxOperations) {
        int L = 1, ans = -1;
        int R = *max_element(nums.begin(), nums.end());
        while(L<=R){
            int M = L + (R-L)/2;
            if(isValid(nums, maxOperations, M)){
                ans = M;
                R = M-1;
            }else
                L = M+1;
        }
        return ans;
    }
};
```