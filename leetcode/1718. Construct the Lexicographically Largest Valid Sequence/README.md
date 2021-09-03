# [1718. Construct the Lexicographically Largest Valid Sequence (Medium)](https://leetcode.com/problems/construct-the-lexicographically-largest-valid-sequence/)

<p>Given an integer <code>n</code>, find a sequence that satisfies all of the following:</p>

<ul>
	<li>The integer <code>1</code> occurs once in the sequence.</li>
	<li>Each integer between <code>2</code> and <code>n</code> occurs twice in the sequence.</li>
	<li>For every integer <code>i</code> between <code>2</code> and <code>n</code>, the <strong>distance</strong> between the two occurrences of <code>i</code> is exactly <code>i</code>.</li>
</ul>

<p>The <strong>distance</strong> between two numbers on the sequence, <code>a[i]</code> and <code>a[j]</code>, is the absolute difference of their indices, <code>|j - i|</code>.</p>

<p>Return <em>the <strong>lexicographically largest</strong> sequence</em><em>. It is guaranteed that under the given constraints, there is always a solution. </em></p>

<p>A sequence <code>a</code> is lexicographically larger than a sequence <code>b</code> (of the same length) if in the first position where <code>a</code> and <code>b</code> differ, sequence <code>a</code> has a number greater than the corresponding number in <code>b</code>. For example, <code>[0,1,9,0]</code> is lexicographically larger than <code>[0,1,5,6]</code> because the first position they differ is at the third number, and <code>9</code> is greater than <code>5</code>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> n = 3
<strong>Output:</strong> [3,1,2,3,2]
<strong>Explanation:</strong> [2,3,2,1,3] is also a valid sequence, but [3,1,2,3,2] is the lexicographically largest valid sequence.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> n = 5
<strong>Output:</strong> [5,3,1,4,3,5,2,4,2]
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 20</code></li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Backtracking](https://leetcode.com/tag/backtracking/)

## Solution 1. Backtracking

```cpp
// OJ: https://leetcode.com/problems/construct-the-lexicographically-largest-valid-sequence/
// Author: A M A N
//Time:  O(N!)
//Space: O(N)
class Solution {
public:

    bool solve(vector<int> &ans, int i, vector<bool> vis){
        if(i==ans.size())
            return true;
        if(ans[i])
            return solve(ans, i+1, vis);
        for(int j = vis.size()-1; j>0; j--){
            if(vis[j])continue;
            if(j!=1 and (i+j>=ans.size() or ans[i+j]))continue;
            vis[j] = 1;
            ans[i] = j;
            if(j!=1) ans[i+j] = j;
            if(solve(ans, i+1, vis))
                return true;
//             this part, this little part is called backtracking
//             coming back from the exact place where you started being incorrect and 
//             eradicating your mistake
            ans[i] = 0;
            if(j!=1) ans[i+j] = 0;
            vis[j] = 0;
        }
        return false;
    }
    vector<int> constructDistancedSequence(int n) {
        vector<int> ans(2*n-1, 0);
        vector<bool> vis(n+1, 0);
        solve(ans, 0, vis);
        return ans;
    }
};
```