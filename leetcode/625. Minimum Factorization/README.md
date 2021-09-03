# [625. Minimum Factorization (Medium)](https://leetcode.com/problems/minimum-factorization/)

<p>Given a positive integer num, return <em>the smallest positive integer </em><code>x</code><em> whose multiplication of each digit equals </em><code>num</code>. If there is no answer or the answer is not fit in <strong>32-bit</strong> signed integer, return <code>0</code>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<pre><strong>Input:</strong> num = 48
<strong>Output:</strong> 68
</pre><p><strong>Example 2:</strong></p>
<pre><strong>Input:</strong> num = 15
<strong>Output:</strong> 35
</pre>
<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= num &lt;= 2<sup>31</sup> - 1</code></li>
</ul>


**Companies**:  
[Tencent](https://leetcode.com/company/tencent)

**Related Topics**:  
[Math](https://leetcode.com/tag/math/), [Greedy](https://leetcode.com/tag/greedy/)

## Solution 1.

```cpp
// OJ: https://leetcode.com/problems/minimum-factorization/
// Author: A M A N
// Time : O(logN)
// Space: O(1)
class Solution {
public:
    int smallestFactorization(int num) {
        if(num<=2)return num; 
        long res = 0;
        for(int i=9, d=0; i>=2 and d<10; i--){ // factors of num can only be of 1 digit
            while(num%i==0 and d<10){ 
                res += i*pow(10, d++);// since we're traversing the factors in reverse order, our res will be the smallest
                num/=i;
            }
        }  
        return num>2?0: res>INT_MAX?0:(int)res;
    }
};
```