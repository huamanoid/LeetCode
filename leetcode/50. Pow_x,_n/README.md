# [50. Pow(x, n) (Medium)](https://leetcode.com/problems/powx-n/)

<p>Implement <a href="http://www.cplusplus.com/reference/valarray/pow/" target="_blank">pow(x, n)</a>, which calculates <code>x</code> raised to the power <code>n</code> (i.e., <code>x<sup>n</sup></code>).</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> x = 2.00000, n = 10
<strong>Output:</strong> 1024.00000
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> x = 2.10000, n = 3
<strong>Output:</strong> 9.26100
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> x = 2.00000, n = -2
<strong>Output:</strong> 0.25000
<strong>Explanation:</strong> 2<sup>-2</sup> = 1/2<sup>2</sup> = 1/4 = 0.25
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>-100.0 &lt;&nbsp;x&nbsp;&lt; 100.0</code></li>
	<li><code>-2<sup>31</sup>&nbsp;&lt;= n &lt;=&nbsp;2<sup>31</sup>-1</code></li>
	<li><code>-10<sup>4</sup> &lt;= x<sup>n</sup> &lt;= 10<sup>4</sup></code></li>
</ul>


**Related Topics**:  
[Math](https://leetcode.com/tag/math/), [Recursion](https://leetcode.com/tag/recursion/)

**Similar Questions**:
* [Sqrt(x) (Easy)](https://leetcode.com/problems/sqrtx/)
* [Super Pow (Medium)](https://leetcode.com/problems/super-pow/)

## Solution 1.

```cpp
// OJ: https://leetcode.com/problems/powx-n/
// Author: A M A N
// Time : O(logN)
// Space: O(1)
class Solution {
public:
    double myPow(double x, int n) {
        bool neg = n<0;
        double ans = 1;
        long N = abs(n); //to handle INT_MIN
        while(N){
            if(N & 1){
                ans = ans*x;
                N--;
            } else {
                x = x*x;
                N/=2;
            }
        }
        return neg ? 1/ans : ans;
    }
};
```