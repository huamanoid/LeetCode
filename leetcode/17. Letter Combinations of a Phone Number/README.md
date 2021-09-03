# [17. Letter Combinations of a Phone Number (Medium)](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)

<p>Given a string containing digits from <code>2-9</code> inclusive, return all possible letter combinations that the number could represent. Return the answer in <strong>any order</strong>.</p>

<p>A mapping of digit to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.</p>

<p><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/7/73/Telephone-keypad2.svg/200px-Telephone-keypad2.svg.png" style="width: 200px; height: 162px;"></p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> digits = "23"
<strong>Output:</strong> ["ad","ae","af","bd","be","bf","cd","ce","cf"]
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> digits = ""
<strong>Output:</strong> []
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> digits = "2"
<strong>Output:</strong> ["a","b","c"]
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>0 &lt;= digits.length &lt;= 4</code></li>
	<li><code>digits[i]</code> is a digit in the range <code>['2', '9']</code>.</li>
</ul>


**Related Topics**:  
[Hash Table](https://leetcode.com/tag/hash-table/), [String](https://leetcode.com/tag/string/), [Backtracking](https://leetcode.com/tag/backtracking/)

**Similar Questions**:
* [Generate Parentheses (Medium)](https://leetcode.com/problems/generate-parentheses/)
* [Combination Sum (Medium)](https://leetcode.com/problems/combination-sum/)
* [Binary Watch (Easy)](https://leetcode.com/problems/binary-watch/)

## Solution 1. Recursion (I/O-Method)

```cpp
// OJ: https://leetcode.com/problems/letter-combinations-of-a-phone-number/
// Author: A M A N
//Time:  O(4^N)
//Space: O(N)
class Solution {
public:

    vector<string> ans;
    vector<string> mp = {"#", "@", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
    void solve(string input, string output){
        if(input.size()==0){
            ans.push_back(output);
            return;
        }
        int last = (input[0]-'0');
        input = input.substr(1);
        for(int i = 0; i < mp[last].size(); i++){
            string output_new = output + (mp[last][i]); 
            solve(input, output_new);
        }
        return;
    }
    vector<string> letterCombinations(string digits) {
        if(!digits.size())return vector<string>();
        solve(digits, "");
        return ans;
    }
};
```