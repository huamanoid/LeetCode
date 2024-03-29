# [997. Find the Town Judge (Easy)](https://leetcode.com/problems/find-the-town-judge/)

<p>In a town, there are <code>n</code> people labelled from&nbsp;<code>1</code> to <code>n</code>.&nbsp; There is a rumor that one of these people is secretly the town judge.</p>

<p>If the&nbsp;town judge exists, then:</p>

<ol>
	<li>The town judge trusts nobody.</li>
	<li>Everybody (except for the town judge) trusts the town judge.</li>
	<li>There is exactly one person that satisfies properties 1 and 2.</li>
</ol>

<p>You are given <code>trust</code>, an array of pairs <code>trust[i] = [a, b]</code> representing that the person labelled <code>a</code> trusts the person labelled <code>b</code>.</p>

<p>If the town judge exists and can be identified, return the label of the town judge.&nbsp; Otherwise, return <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> n = 2, trust = [[1,2]]
<strong>Output:</strong> 2
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> n = 3, trust = [[1,3],[2,3]]
<strong>Output:</strong> 3
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> n = 3, trust = [[1,3],[2,3],[3,1]]
<strong>Output:</strong> -1
</pre>

<p><strong>Example 4:</strong></p>

<pre><strong>Input:</strong> n = 3, trust = [[1,2],[2,3]]
<strong>Output:</strong> -1
</pre>

<p><strong>Example 5:</strong></p>

<pre><strong>Input:</strong> n = 4, trust = [[1,3],[1,4],[2,3],[2,4],[4,3]]
<strong>Output:</strong> 3
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 1000</code></li>
	<li><code>0 &lt;= trust.length &lt;= 10<sup>4</sup></code></li>
	<li><code>trust[i].length == 2</code></li>
	<li><code>trust[i]</code> are all different</li>
	<li><code>trust[i][0] != trust[i][1]</code></li>
	<li><code>1 &lt;= trust[i][0], trust[i][1] &lt;= n</code></li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Hash Table](https://leetcode.com/tag/hash-table/), [Graph](https://leetcode.com/tag/graph/)

**Similar Questions**:
* [Find the Celebrity (Medium)](https://leetcode.com/problems/find-the-celebrity/)

## Solution 1.

```cpp
// OJ: https://leetcode.com/problems/find-the-town-judge/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    int findJudge(int n, vector<vector<int>>& trust) {
        unordered_map<int, int> in, out;
        for(auto e: trust)
            out[e[0]]++, in[e[1]]++;
        for(int i=1; i<=n; i++)
            if(in[i]==n-1 and out[i]==0)
                return i;
        return -1;
    }
};
```