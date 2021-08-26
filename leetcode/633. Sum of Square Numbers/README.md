# [633. Sum of Square Numbers (Medium)](https://leetcode.com/problems/sum-of-square-numbers/)

<p>Given a non-negative integer <code>c</code>, decide whether there're two integers <code>a</code> and <code>b</code> such that <code>a<sup>2</sup> + b<sup>2</sup> = c</code>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> c = 5
<strong>Output:</strong> true
<strong>Explanation:</strong> 1 * 1 + 2 * 2 = 5
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> c = 3
<strong>Output:</strong> false
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> c = 4
<strong>Output:</strong> true
</pre>

<p><strong>Example 4:</strong></p>

<pre><strong>Input:</strong> c = 2
<strong>Output:</strong> true
</pre>

<p><strong>Example 5:</strong></p>

<pre><strong>Input:</strong> c = 1
<strong>Output:</strong> true
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>0 &lt;= c &lt;= 2<sup>31</sup> - 1</code></li>
</ul>


**Companies**:  
[Apple](https://leetcode.com/company/apple)

**Related Topics**:  
[Math](https://leetcode.com/tag/math/), [Two Pointers](https://leetcode.com/tag/two-pointers/), [Binary Search](https://leetcode.com/tag/binary-search/)

**Similar Questions**:
* [Valid Perfect Square (Easy)](https://leetcode.com/problems/valid-perfect-square/)

## Solution 1.

```cpp
// OJ: https://leetcode.com/problems/sum-of-square-numbers/
// Author: A M A N
// Time : O(sqrt(C)*logC)
// Space: O(1)
class Solution {
public:
    bool isSquare(int n){
        return (int)sqrt(n)*(int)sqrt(n)==n;
    }
    bool judgeSquareSum(int c) {
        for(int64_t i=0; i*i<=c; i++){
            if(isSquare(c-i*i)){
                return true;
            }
        }
        return false;
    }
};
```