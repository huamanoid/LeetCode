# [984. String Without AAA or BBB (Medium)](https://leetcode.com/problems/string-without-aaa-or-bbb/)

<p>Given two integers <code>a</code> and <code>b</code>, return <strong>any</strong> string <code>s</code> such that:</p>

<ul>
	<li><code>s</code> has length <code>a + b</code> and contains exactly <code>a</code> <code>'a'</code> letters, and exactly <code>b</code> <code>'b'</code> letters,</li>
	<li>The substring <code>'aaa'</code> does not occur in <code>s</code>, and</li>
	<li>The substring <code>'bbb'</code> does not occur in <code>s</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> a = 1, b = 2
<strong>Output:</strong> "abb"
<strong>Explanation:</strong> "abb", "bab" and "bba" are all correct answers.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> a = 4, b = 1
<strong>Output:</strong> "aabaa"
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>0 &lt;= a, b &lt;= 100</code></li>
	<li>It is guaranteed such an <code>s</code> exists for the given <code>a</code> and <code>b</code>.</li>
</ul>


**Companies**:  
[Grab](https://leetcode.com/company/grab)

**Related Topics**:  
[String](https://leetcode.com/tag/string/), [Greedy](https://leetcode.com/tag/greedy/)

## Solution 1.

![](https://assets.leetcode.com/users/easternbadge/image_1581662561.png)

```cpp
// OJ: https://leetcode.com/problems/string-without-aaa-or-bbb/
// Author: A M A N
// Time : O(A+B)
// Space: O(A+B)
// Ref  : https://leetcode.com/problems/string-without-aaa-or-bbb/discuss/508543/APPLES-and-BANANAS-solution-(with-picture)
class Solution {
public:
    string strWithout3a3b(int a, int b) {
        char charToLayDown, charToTopUp;
        int numberToLayDown, numberToTopUp;
        if(a>b){
            charToLayDown = 'a';
            charToTopUp   = 'b';
            numberToLayDown = a;
            numberToTopUp   = b;
        }else{
            charToLayDown = 'b';
            charToTopUp   = 'a';
            numberToLayDown = b;
            numberToTopUp   = a;
        }
        int repitition = numberToLayDown/2;
        int remainder  = numberToLayDown%2;
        vector<string> v;
        for(int i=0; i<repitition; i++)
            v.push_back(string(2, charToLayDown));
        if(remainder)
            v.push_back(string(1, charToLayDown));
        int n = v.size();
        int idx = 0;
        while(numberToTopUp>0){
            v[idx] += charToTopUp;
            idx = (idx+1)%n;
            numberToTopUp--;
        }
        string ans;
        for(auto s: v)
            ans+=s;
        return ans;
    }
};
```

## Solution 2. Greedy

```cpp
// OJ: https://leetcode.com/problems/string-without-aaa-or-bbb/
// Author: A M A N
// Time : O(A+B)
// Space: O(1)
class Solution {
public:
    string strWithout3a3b(int A, int B) {
        char a = 'a', b = 'b';
        if (A < B) swap(A, B), swap(a, b);
        string ans;
        while (A && B) {
            for (int i = A > B ? 2 : 1; i > 0; --i, --A) 
                ans += a;
            ans += b;
            --B;
        }
        while (A-- > 0) ans += a;
        return ans;
    }
};
```