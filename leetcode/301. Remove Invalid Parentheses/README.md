# [301. Remove Invalid Parentheses (Hard)](https://leetcode.com/problems/remove-invalid-parentheses/)

<p>Given a string <code>s</code> that contains parentheses and letters, remove the minimum number of invalid parentheses to make the input string valid.</p>

<p>Return <em>all the possible results</em>. You may return the answer in <strong>any order</strong>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> s = "()())()"
<strong>Output:</strong> ["(())()","()()()"]
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> s = "(a)())()"
<strong>Output:</strong> ["(a())()","(a)()()"]
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> s = ")("
<strong>Output:</strong> [""]
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 25</code></li>
	<li><code>s</code> consists of lowercase English letters and parentheses <code>'('</code> and <code>')'</code>.</li>
	<li>There will be at most <code>20</code> parentheses in <code>s</code>.</li>
</ul>


**Related Topics**:  
[String](https://leetcode.com/tag/string/), [Backtracking](https://leetcode.com/tag/backtracking/), [Breadth-First Search](https://leetcode.com/tag/breadth-first-search/)

**Similar Questions**:
* [Valid Parentheses (Easy)](https://leetcode.com/problems/valid-parentheses/)

## Solution 1. Backtracking (TLE)

```cpp
// OJ: https://leetcode.com/problems/remove-invalid-parentheses/
// Author: A M A N
// Time : O(2 ^ N)
// Space: O(N)
class Solution {
public:
    unordered_set<string> ans;
    bool isValid(string temp){
        stack<char>st;
        for(char c: temp){
            if(c=='(')
                st.push('(');
            else if(c==')'){
                if(st.empty()){
                    return false;
                }else 
                    st.pop();
            }
        }
        return st.empty();
    }
    void dfs(string s, int fromLast, string temp){
        if(isValid(temp)){
            ans.insert(temp);
            return;
        }
        if(fromLast<0)
            return;
        string s1 = temp;
        int i = temp.size() - fromLast - 1;
        string s2 = temp.substr(0, i) + temp.substr(i+1);
        dfs(s, fromLast-1, s1);
        dfs(s, fromLast-1, s2);
    }
    vector<string> removeInvalidParentheses(string s) {
        dfs(s, s.size()-1, s);        
        vector<string> res;
        int cnt=0;
        for(auto s: ans)
            cnt = max(cnt, (int)s.size());
        for(auto s: ans)
            if(s.size()==cnt)
                res.push_back(s);
        return res;
    }
};
```


## Solution 2. Backtracking 

```cpp
// OJ: https://leetcode.com/problems/remove-invalid-parentheses/
// Author: A M A N
// Time : O(2 ^ N)
// Space: O(N)
class Solution {
public:
    vector<string> ans;
    bool isValid(string temp){
        stack<char>st;
        for(char c: temp){
            if(c=='(')
                st.push('(');
            else if(c==')'){
                if(st.empty()){
                    return false;
                }else 
                    st.pop();
            }
        }
        return st.empty();
    }
    void dfs(string s, int start, int left, int right){
        if(left==0 and right==0)
            if(isValid(s)){
                ans.push_back(s);
                return;
            }
        for(int i=start; i<s.size(); i++){
            string temp = s;
            if(left>0 and temp[i]=='('){
                if(i==start or temp[i]!=temp[i-1]){ // Duplicates arise from consecutive '(' or ')'
                    temp.erase(i, 1);
                    dfs(temp, i, left-1, right); // notice index is not increased because one char is deleted.
                }
            }
            if(right>0 and temp[i]==')'){
                if(i==start or temp[i]!=temp[i-1]){ // Duplicates arise from consecutive '(' or ')'
                    temp.erase(i, 1);
                    dfs(temp, i, left, right-1);
                }
            }
        }
    }
    vector<string> removeInvalidParentheses(string s) {
        int left=0, right=0; //count of misplaced '(' and ')' brackets
        for(char c: s){
            if(c=='(') left++;
            else if(c==')'){
                if(left) left--;
                else    right++;
            }
        }
        dfs(s, 0, left, right);
        return ans;
    }
};
```