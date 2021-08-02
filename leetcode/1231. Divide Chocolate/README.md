# [1231. Divide Chocolate (Hard)](https://www.lintcode.com/problem/1817/description)

<p>You have one chocolate bar that consists of some chunks. Each chunk has its own sweetness given by the array sweetness.</p>

<p>You're going to share this chocolate with K friends, so you need to cut K times to get K + 1 pieces, each of which is made up of a series of small pieces.</p>

<p>Being generous, you will eat the piece with the minimum total sweetness and give the other pieces to your friends.</p>

<p>Find the <strong>maximum</strong> total sweetness of the piece you can get by cutting the chocolate bar optimally.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<pre><strong>Input:</strong> sweetness = [1,2,3,4,5,6,7,8,9], K = 5
<strong>Output:</strong> 6
<strong>Explanation:</strong> You can divide the chocolate to [1,2,3], [4,5], [6], [7], [8], [9]
</pre><p><strong>Example 2:</strong></p>
<pre><strong>Input:</strong> sweetness = [5,6,7,8,9,1,2,3,4], K = 8
<strong>Output:</strong> 1
Explanation: There is only one way to cut the bar into 9 pieces.
</pre><p><strong>Example 3:</strong></p>
<pre><strong>Input:</strong> sweetness = [1,2,2,1,2,2,1,2,2], K = 2
<strong>Output:</strong> 5
Explanation: You can divide the chocolate to [1,2,2], [1,2,2], [1,2,2]
</pre>
<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>0 &lt;= K < sweetness.length  &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= sweetness[i] &lt;= 10<sup>5</sup></code></li>
</ul>




## Solution 1. Binary Search on answers

```cpp
// OJ: https://leetcode.com/problems/divide-chocolate/
// Author: A M A N
// Time: O(N*logS) where S is the sum of elements in the array
// Space: O(1)
class Solution {
public:
    bool isValid(vector<int>nums, int totalCuts, int min){
        int sum=0, cuts=0;
        for(int i=0; i<nums.size(); i++){
            sum+=nums[i];
            if(sum>=min and cuts!=totalCuts){ // since the sum is greater than the min allowed we can cut it here
                sum = 0;
                cuts++;
            }
        }
        if(sum<min or cuts!=totalCuts)return false;
        if(sum>=min) return true;
        return false;
    }
    int maximizeSweetness(vector<int> &sweetness, int K) {
        int ans = -1;
        int L = 1;
        int R = accumulate(sweetness.begin(), sweetness.end(), 0);
        while(L<=R){
            int M = L + (R-L)/2;
            if(isValid(sweetness, K, M)){
                ans = M;
                L = M+1;
            }else
                R = M-1;
        }
        return ans;
    }
};
```