# [1052. Grumpy Bookstore Owner (Medium)](https://leetcode.com/problems/grumpy-bookstore-owner/)

<p>There is a bookstore owner that has a store open for <code>n</code> minutes. Every minute, some number of customers enter the store. You are given an integer array <code>customers</code> of length <code>n</code> where <code>customers[i]</code> is the number of the customer that enters the store at the start of the <code>i<sup>th</sup></code> minute and all those customers leave after the end of that minute.</p>

<p>On some minutes, the bookstore owner is grumpy. You are given a binary array grumpy where <code>grumpy[i]</code> is <code>1</code> if the bookstore owner is grumpy during the <code>i<sup>th</sup></code> minute, and is <code>0</code> otherwise.</p>

<p>When the bookstore owner is grumpy, the customers of that minute are not satisfied, otherwise, they are satisfied.</p>

<p>The bookstore owner knows a secret technique to keep themselves not grumpy for <code>minutes</code> consecutive minutes, but can only use it once.</p>

<p>Return <em>the maximum number of customers that can be satisfied throughout the day</em>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> customers = [1,0,1,2,1,1,7,5], grumpy = [0,1,0,1,0,1,0,1], minutes = 3
<strong>Output:</strong> 16
<strong>Explanation:</strong> The bookstore owner keeps themselves not grumpy for the last 3 minutes. 
The maximum number of customers that can be satisfied = 1 + 1 + 1 + 1 + 7 + 5 = 16.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> customers = [1], grumpy = [0], minutes = 1
<strong>Output:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>n == customers.length == grumpy.length</code></li>
	<li><code>1 &lt;= minutes &lt;= n &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>0 &lt;= customers[i] &lt;= 1000</code></li>
	<li><code>grumpy[i]</code> is either <code>0</code> or <code>1</code>.</li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Sliding Window](https://leetcode.com/tag/sliding-window/)

## Solution 1. Sliding Window

The problem is similar to max sum of subarray of length K but with one constraint. 

```cpp
// OJ: https://leetcode.com/problems/grumpy-bookstore-owner/
// Author: A M A N
// Time : O(N)
// Space: O(1)
class Solution {
public:
    int maxSatisfied(vector<int>& customers, vector<int>& grumpy, int minutes) {
        // we need to find the pos from where the "secret technique" can be applied, we call it "start".
        // calculate max sum [only when he's grumpy] of a subarray of fixed length = minutes 
        int i=0, j=0, sum=0, start=-1, maxSum = 0;
        while(j<customers.size()){
            sum += (grumpy[j]?customers[j]:0);
            int window = j-i+1;
            if(window<minutes)
                j++;
            else if(window==minutes){
                if(maxSum<sum){
                    start = i;
                    maxSum = sum;
                }
                if(grumpy[i])
                    sum-=customers[i]; // subtract only if you added it in the first place
                i++, j++; // sliding the window, maintaining the length
            }
        }
        // we calculated max customers which are secretly satisfied, now just add the directly satisfied customers.
        int res = 0;
        for(int i=0; i<customers.size(); i++){
            if(i==start){ //secretly satisfied
                res += accumulate(customers.begin()+start, customers.begin()+start+minutes, 0);
                i   += minutes-1; // since i+1 will happen at the end of the loop
            }else   // directly satisfied
                res+= (grumpy[i]?0:customers[i]);
        }
        return res;
    }
};
```