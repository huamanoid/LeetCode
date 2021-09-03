# [32. Longest Valid Parentheses (Hard)](https://leetcode.com/problems/longest-valid-parentheses/)

<p>Given a string containing just the characters <code>'('</code> and <code>')'</code>, find the length of the longest valid (well-formed) parentheses substring.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> s = "(()"
<strong>Output:</strong> 2
<strong>Explanation:</strong> The longest valid parentheses substring is "()".
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> s = ")()())"
<strong>Output:</strong> 4
<strong>Explanation:</strong> The longest valid parentheses substring is "()()".
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> s = ""
<strong>Output:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>0 &lt;= s.length &lt;= 3 * 10<sup>4</sup></code></li>
	<li><code>s[i]</code> is <code>'('</code>, or <code>')'</code>.</li>
</ul>


**Related Topics**:  
[String](https://leetcode.com/tag/string/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/), [Stack](https://leetcode.com/tag/stack/)

**Similar Questions**:
* [Valid Parentheses (Easy)](https://leetcode.com/problems/valid-parentheses/)

## Solution 1. DP

```cpp
// OJ: https://leetcode.com/problems/longest-valid-parentheses/
// Author: A M A N
// Time : O(N)
// Space: O(N)
// Ref  : https://leetcode.com/problems/longest-valid-parentheses/solution/
class Solution {
public:
    int longestValidParentheses(string s) {
        int n = s.size();
        vector<int>dp(n, 0);
        for(int i=1; i<n; i++){
            if(s[i]==')'){
                if(s[i-1]=='(')
                    dp[i] = (i >= 2 ? dp[i-2] : 0) + 2;
                if(s[i-1]==')' and i-dp[i-1]>0 and s[i-dp[i-1]-1]=='(')
                    dp[i] = dp[i-1] + ((i-dp[i-1])>=2 ? dp[i-dp[i-1]-2] : 0) + 2;
            }
        }
        return n?*max_element(dp.begin(), dp.end()):0;
    }
};
```


## Solution 2. Stack

```cpp
// OJ: https://leetcode.com/problems/longest-valid-parentheses/solution/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    int longestValidParentheses(string s) {
        stack<int> st;
        st.push(-1);
        int ans = 0;
        for(int i=0; i<s.size(); i++){
            if(s[i]=='(')
                st.push(i);
            else{
                st.pop();
                if(st.empty())
                    st.push(i); // pushing the index to calculate the valid length aftwerwards
                else ans = max(ans, i - st.top());
            }
        }
        return ans;
    }
};
```
More intuitive method :
>  Just mark the positions where the braces are valid and then calculate the longest valid...
```cpp
// OJ: https://leetcode.com/problems/longest-valid-parentheses/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    int longest(vector<bool>& valid){
        int res = 0;
        int curLength = 0;
        for(bool v: valid)
            res = max(res, curLength = (v ? curLength+1 : 0));
        return res;
    }
    int longestValidParentheses(string s) {
        vector<bool> valid(s.size(), false);
        stack<int> st;
        for(int i=0; i<s.size(); i++){
            if(s[i]=='(') st.push(i);
            else if(!st.empty()){
                int open = st.top(); st.pop();
                valid[open] = valid[i] = true; // marking the open and corresponding close (i) brackets as true;
            }
        }
        return longest(valid);
    }
};
```