# [740. Delete and Earn (Medium)](https://leetcode.com/problems/delete-and-earn/)

<p>You are given an integer array <code>nums</code>. You want to maximize the number of points you get by performing the following operation any number of times:</p>

<ul>
	<li>Pick any <code>nums[i]</code> and delete it to earn <code>nums[i]</code> points. Afterwards, you must delete <b>every</b> element equal to <code>nums[i] - 1</code> and <strong>every</strong> element equal to <code>nums[i] + 1</code>.</li>
</ul>

<p>Return <em>the <strong>maximum number of points</strong> you can earn by applying the above operation some number of times</em>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> nums = [3,4,2]
<strong>Output:</strong> 6
<strong>Explanation:</strong> You can perform the following operations:
- Delete 4 to earn 4 points. Consequently, 3 is also deleted. nums = [2].
- Delete 2 to earn 2 points. nums = [].
You earn a total of 6 points.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> nums = [2,2,3,3,3,4]
<strong>Output:</strong> 9
<strong>Explanation:</strong> You can perform the following operations:
- Delete a 3 to earn 3 points. All 2's and 4's are also deleted. nums = [3,3].
- Delete a 3 again to earn 3 points. nums = [3].
- Delete a 3 once more to earn 3 points. nums = [].
You earn a total of 9 points.</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>4</sup></code></li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Hash Table](https://leetcode.com/tag/hash-table/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/)

**Similar Questions**:
* [House Robber (Medium)](https://leetcode.com/problems/house-robber/)

## Solution 1. Recursion + Memoization

```cpp
// OJ: https://leetcode.com/problems/delete-and-earn/
// Author: A M A N
// Time : O(N*logN)
// Space: O(N)
class Solution {
public:
    unordered_map<int, int> dp;
    int solve(vector<int>& nums, int i, int n){
        if(i>=n)return 0;
        if(dp.find(i)!=dp.end())
            return dp[i];
        int val  = nums[i];
        int next = upper_bound(nums.begin(), nums.end(), val) - nums.begin(); // next element if val+1 does not exist
        int sum  = (next-i)*val; //Once we decide that we want a num, we can add all the occurrences of num into the total
        int k = next; 
        if(next<n and nums[next]==val+1){
            next = upper_bound(nums.begin(), nums.end(), val+1) - nums.begin(); //next element if val+1 exist
        }
        return dp[i] = max(sum + solve(nums, next, n), solve(nums, k, n));
    }
    int deleteAndEarn(vector<int>& nums) {
        sort(nums.begin(), nums.end()); //sorting reduces the task to remove additional elements in one direction only
        return solve(nums, 0, nums.size());
    }
};
```


## Solution 2. DP

Similar to [House Robber (Medium)](https://leetcode.com/problems/house-robber/) problem 

```cpp
// OJ: https://leetcode.com/problems/delete-and-earn/
// Author: A M A N
// Time : O(N + R) where R is the upper limit of allowable values of nums[i]
// Space: O(N + R)
class Solution {
public:
    int deleteAndEarn(vector<int>& nums){
        unordered_map<int, int> dp; // dp[element] = points (sum of those elements)
        for(auto e: nums)
            dp[e]+=e;
        int ans = dp[1];
        for(int i=2; i<=10000; i++){
            dp[i] = max(dp[i-1], dp[i-2] + dp[i]); // skip, pick
            ans = max(dp[i], ans);
        }
        return ans;
    }
};
```