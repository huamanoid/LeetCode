# [784. Letter Case Permutation (Medium)](https://leetcode.com/problems/letter-case-permutation/)

<p>Given a string <code>s</code>, we can transform every letter individually to be lowercase or uppercase to create another string.</p>

<p>Return <em>a list of all possible strings we could create</em>. You can return the output&nbsp;in <strong>any order</strong>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> s = "a1b2"
<strong>Output:</strong> ["a1b2","a1B2","A1b2","A1B2"]
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> s = "3z4"
<strong>Output:</strong> ["3z4","3Z4"]
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> s = "12345"
<strong>Output:</strong> ["12345"]
</pre>

<p><strong>Example 4:</strong></p>

<pre><strong>Input:</strong> s = "0"
<strong>Output:</strong> ["0"]
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>s</code> will be a string with length between <code>1</code> and <code>12</code>.</li>
	<li><code>s</code> will consist only of letters or digits.</li>
</ul>


**Related Topics**:  
[String](https://leetcode.com/tag/string/), [Backtracking](https://leetcode.com/tag/backtracking/), [Bit Manipulation](https://leetcode.com/tag/bit-manipulation/)

**Similar Questions**:
* [Subsets (Medium)](https://leetcode.com/problems/subsets/)
* [Brace Expansion (Medium)](https://leetcode.com/problems/brace-expansion/)

## Solution 1. Recursion (I/O Method) 

```cpp
// OJ: https://leetcode.com/problems/letter-case-permutation/
// Author: A M A N
// Time:  O(2^N)
// Space: O(N)
class Solution {
public:
    vector<string> ans;
    void solve(string input, string output){
        if(input.size()==0){
            ans.push_back(output);
            return;
        }
        char temp = input[0];
        input = input.substr(1);
        if(!isalpha(temp)){
            output += temp;
//             ONLY 1 branch in Recursion tree when there is a digit. 
            solve(input, output);
        }else{
            string output1 = output+temp;
            temp = toupper(temp);
            string output2 = output + temp;
            solve(input, output1);
            solve(input, output2);
        }
        return;
    }
    vector<string> letterCasePermutation(string s) {
        string output = "";
        transform(s.begin(), s.end(), s.begin(), ::tolower);
        solve(s, output);
        return ans;
    }
};
```