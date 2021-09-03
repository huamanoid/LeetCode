# [22. Generate Parentheses (Medium)](https://leetcode.com/problems/generate-parentheses/)

<p>Given <code>n</code> pairs of parentheses, write a function to <em>generate all combinations of well-formed parentheses</em>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<pre><strong>Input:</strong> n = 3
<strong>Output:</strong> ["((()))","(()())","(())()","()(())","()()()"]
</pre><p><strong>Example 2:</strong></p>
<pre><strong>Input:</strong> n = 1
<strong>Output:</strong> ["()"]
</pre>
<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 8</code></li>
</ul>


**Related Topics**:  
[String](https://leetcode.com/tag/string/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/), [Backtracking](https://leetcode.com/tag/backtracking/)

**Similar Questions**:
* [Letter Combinations of a Phone Number (Medium)](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)
* [Valid Parentheses (Easy)](https://leetcode.com/problems/valid-parentheses/)

## Solution 1. Backtracking

```cpp
// OJ: https://leetcode.com/problems/generate-parentheses/
// Author: A M A N
// Time:  O(C(N)) where C(N) is the Nth Catalan number
// Space: O(N)
class Solution {
public:

    vector<string> ans;
    void solve(string output, int open, int close){
        if(open==0 and close==0){
            ans.push_back(output);
            return;
        }
        string output1 = output + "(";
        string output2 = output + ")";
        if(open){
            solve(output1, open-1, close);
        }
        if(close>open){
            solve(output2, open, close-1);
        }
        return;
    }
    vector<string> generateParenthesis(int n) {
        string output;
        solve(output, n, n);    
        return ans;
    }
};
```