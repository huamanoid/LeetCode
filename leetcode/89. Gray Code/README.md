# [89. Gray Code (Medium)](https://leetcode.com/problems/gray-code/)

<p>An <strong>n-bit gray code sequence</strong> is a sequence of <code>2<sup>n</sup></code> integers where:</p>

<ul>
	<li>Every integer is in the <strong>inclusive</strong> range <code>[0, 2<sup>n</sup> - 1]</code>,</li>
	<li>The first integer is <code>0</code>,</li>
	<li>An integer appears <strong>no more than once</strong> in the sequence,</li>
	<li>The binary representation of every pair of <strong>adjacent</strong> integers differs by <strong>exactly one bit</strong>, and</li>
	<li>The binary representation of the <strong>first</strong> and <strong>last</strong> integers differs by <strong>exactly one bit</strong>.</li>
</ul>

<p>Given an integer <code>n</code>, return <em>any valid <strong>n-bit gray code sequence</strong></em>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> n = 2
<strong>Output:</strong> [0,1,3,2]
<strong>Explanation:</strong>
The binary representation of [0,1,3,2] is [00,01,11,10].
- 0<u>0</u> and 0<u>1</u> differ by one bit
- <u>0</u>1 and <u>1</u>1 differ by one bit
- 1<u>1</u> and 1<u>0</u> differ by one bit
- <u>1</u>0 and <u>0</u>0 differ by one bit
[0,2,3,1] is also a valid gray code sequence, whose binary representation is [00,10,11,01].
- <u>0</u>0 and <u>1</u>0 differ by one bit
- 1<u>0</u> and 1<u>1</u> differ by one bit
- <u>1</u>1 and <u>0</u>1 differ by one bit
- 0<u>1</u> and 0<u>0</u> differ by one bit
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> n = 1
<strong>Output:</strong> [0,1]
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 16</code></li>
</ul>


**Companies**:  
[Microsoft](https://leetcode.com/company/microsoft)

**Related Topics**:  
[Math](https://leetcode.com/tag/math/), [Backtracking](https://leetcode.com/tag/backtracking/), [Bit Manipulation](https://leetcode.com/tag/bit-manipulation/)

**Similar Questions**:
* [1-bit and 2-bit Characters (Easy)](https://leetcode.com/problems/1-bit-and-2-bit-characters/)

## Solution 1. Backtracking

```cpp
// OJ: https://leetcode.com/problems/gray-code/
// Author: A M A N
// Time : O((2^N) * N + (2^N) * log(2^N))
// Space: O(2^N)
class Solution {
public:
    vector<vector<int>> ans;
    set<vector<int>> vis;
    void solve(vector<int>temp){
        vis.insert(temp);
        ans.push_back(temp); // to preserve the sequence
        for(int i=temp.size()-1; i>=0; i--){ // traversing backwards for gray code sequence
            temp[i]^=1;
            if(!vis.count(temp))
                solve(temp);
            temp[i]^=1; //backtrack
        }
    }
    vector<int> grayCode(int n) {
        vector<int> temp(n, 0);
        solve(temp);
        return convertToDecimals(ans); // converts vector of binary values to decimals
    }
private:
    vector<int> convertToDecimals(vector<vector<int>>&ans){
        vector<int> res;
        for(auto v: ans){
            int num=0, n = v.size();
            for(int i=n-1; i>=0; i--)
                num += v[i]*(1<<(n-1-i));
            res.push_back(num);
        }
        return res;
    }
};
```

TODO: using bits instead of vectors