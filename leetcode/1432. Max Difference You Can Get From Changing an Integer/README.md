# [1432. Max Difference You Can Get From Changing an Integer (Medium)](https://leetcode.com/problems/max-difference-you-can-get-from-changing-an-integer/)

<p>You are given an integer <code>num</code>. You will apply the following steps exactly <strong>two</strong> times:</p>

<ul>
	<li>Pick a digit <code>x (0&nbsp;&lt;= x &lt;= 9)</code>.</li>
	<li>Pick another digit <code>y (0&nbsp;&lt;= y &lt;= 9)</code>. The digit <code>y</code> can be equal to <code>x</code>.</li>
	<li>Replace all the occurrences of <code>x</code> in the decimal representation of <code>num</code> by <code>y</code>.</li>
	<li>The new integer <strong>cannot</strong> have any leading zeros, also the new integer <strong>cannot</strong> be 0.</li>
</ul>

<p>Let <code>a</code>&nbsp;and <code>b</code>&nbsp;be the results of applying the operations to <code>num</code> the first and second times, respectively.</p>

<p>Return <em>the max difference</em> between <code>a</code> and <code>b</code>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> num = 555
<strong>Output:</strong> 888
<strong>Explanation:</strong> The first time pick x = 5 and y = 9 and store the new integer in a.
The second time pick x = 5 and y = 1 and store the new integer in b.
We have now a = 999 and b = 111 and max difference = 888
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> num = 9
<strong>Output:</strong> 8
<strong>Explanation:</strong> The first time pick x = 9 and y = 9 and store the new integer in a.
The second time pick x = 9 and y = 1 and store the new integer in b.
We have now a = 9 and b = 1 and max difference = 8
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> num = 123456
<strong>Output:</strong> 820000
</pre>

<p><strong>Example 4:</strong></p>

<pre><strong>Input:</strong> num = 10000
<strong>Output:</strong> 80000
</pre>

<p><strong>Example 5:</strong></p>

<pre><strong>Input:</strong> num = 9288
<strong>Output:</strong> 8700
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= num &lt;= 10^8</code></li>
</ul>


**Companies**:  
[Mercari](https://leetcode.com/company/mercari)

**Related Topics**:  
[Math](https://leetcode.com/tag/math/), [Greedy](https://leetcode.com/tag/greedy/)

## Solution 1.

```cpp
// OJ: https://leetcode.com/problems/max-difference-you-can-get-from-changing-an-integer/
// Author: A M A N
// Time : O()
// Space: O()
};
class Solution {
public:
    int convertMax(int num){
        string s = to_string(num);
        char c;
        for(int i=0; i<s.size(); i++){
            if(s[i]!='9'){
                c = s[i];
                break;
            }
        }
        for(int i=0; i<s.size(); i++)
            if(s[i]==c) s[i] = '9'; 
        return stoi(s);
    }
    int convertMin(int num){
        string s = to_string(num);
        char c;
        bool first = false;
        for(int i=0; i<s.size(); i++){
            if(s[i]!='0' and s[i]!='1'){
                if(i==0) first = true;
                c = s[i];
                break;
            }
        }
        if(first){
            for(int i=0; i<s.size(); i++)
                if(s[i]==c) s[i] = '1';
        }else
            for(int i=0; i<s.size(); i++)
                if(s[i]==c) s[i] = '0';
        return stoi(s);
    }
    int maxDiff(int num) {
        int a = convertMax(num);
        int b = convertMin(num);
        return a-b;
    }
};
```