# [556. Next Greater Element III (Medium)](https://leetcode.com/problems/next-greater-element-iii/)

<p>Given a positive integer <code>n</code>, find <em>the smallest integer which has exactly the same digits existing in the integer</em> <code>n</code> <em>and is greater in value than</em> <code>n</code>. If no such positive integer exists, return <code>-1</code>.</p>

<p><strong>Note</strong> that the returned integer should fit in <strong>32-bit integer</strong>, if there is a valid answer but it does not fit in <strong>32-bit integer</strong>, return <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<pre><strong>Input:</strong> n = 12
<strong>Output:</strong> 21
</pre><p><strong>Example 2:</strong></p>
<pre><strong>Input:</strong> n = 21
<strong>Output:</strong> -1
</pre>
<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 2<sup>31</sup> - 1</code></li>
</ul>


**Companies**:  
[Facebook](https://leetcode.com/company/facebook), [Microsoft](https://leetcode.com/company/microsoft)

**Related Topics**:  
[Math](https://leetcode.com/tag/math/), [Two Pointers](https://leetcode.com/tag/two-pointers/), [String](https://leetcode.com/tag/string/)

**Similar Questions**:
* [Next Greater Element I (Easy)](https://leetcode.com/problems/next-greater-element-i/)
* [Next Greater Element II (Medium)](https://leetcode.com/problems/next-greater-element-ii/)
* [Next Palindrome Using Same Digits (Hard)](https://leetcode.com/problems/next-palindrome-using-same-digits/)

## Solution 1.

```cpp
// OJ: https://leetcode.com/problems/next-greater-element-iii/
// Author: A M A N
// Time : O(digits(N))
// Space: O(digits(N))
class Solution {
public:
    int nextGreaterElement(int n) {
        vector<int> v;
        int temp = n;
        while(temp){
            v.push_back(temp%10);
            temp/=10;
        }
        reverse(v.begin(), v.end());
        next_permutation(v.begin(), v.end());
        int64_t ans = 0;
        for(auto a: v)
            ans*=10, ans+=a;
        if(ans<=n or ans>INT_MAX)return -1;
        else return (int)ans;
    }
};
```