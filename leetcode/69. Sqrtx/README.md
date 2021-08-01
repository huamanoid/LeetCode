# [69. Sqrt(x) (Easy)](https://leetcode.com/problems/sqrtx/)

<p>Given a non-negative integer <code>x</code>,&nbsp;compute and return <em>the square root of</em> <code>x</code>.</p>

<p>Since the return type&nbsp;is an integer, the decimal digits are <strong>truncated</strong>, and only <strong>the integer part</strong> of the result&nbsp;is returned.</p>

<p><strong>Note:&nbsp;</strong>You are not allowed to use any built-in exponent function or operator, such as <code>pow(x, 0.5)</code> or&nbsp;<code>x ** 0.5</code>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> x = 4
<strong>Output:</strong> 2
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> x = 8
<strong>Output:</strong> 2
<strong>Explanation:</strong> The square root of 8 is 2.82842..., and since the decimal part is truncated, 2 is returned.</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>0 &lt;= x &lt;= 2<sup>31</sup> - 1</code></li>
</ul>


**Related Topics**:  
[Math](https://leetcode.com/tag/math/), [Binary Search](https://leetcode.com/tag/binary-search/)

**Similar Questions**:
* [Pow(x, n) (Medium)](https://leetcode.com/problems/powx-n/)
* [Valid Perfect Square (Easy)](https://leetcode.com/problems/valid-perfect-square/)

## Solution 1. Binary Search

```cpp
// OJ: https://leetcode.com/problems/sqrtx/
// Author: A M A N
// Time : O(logN)
// Space: O(1)
class Solution {
public:
    int mySqrt(int num) {
        int start = 1;
        int end = num;
        int ans = 0;
        while(start<=end){
            int mid = start + (end-start)/2;
            if(mid<=(num/mid)){   
                ans = mid;         //candidate for ans
                start = mid+1;
            }
            else
                end = mid-1;
        }
        return ans;
    }
};
```