# [1626. Best Team With No Conflicts (Medium)](https://leetcode.com/problems/best-team-with-no-conflicts/)

<p>You are the manager of a basketball team. For the upcoming tournament, you want to choose the team with the highest overall score. The score of the team is the <strong>sum</strong> of scores of all the players in the team.</p>

<p>However, the basketball team is not allowed to have <strong>conflicts</strong>. A <strong>conflict</strong> exists if a younger player has a <strong>strictly higher</strong> score than an older player. A conflict does <strong>not</strong> occur between players of the same age.</p>

<p>Given two lists, <code>scores</code> and <code>ages</code>, where each <code>scores[i]</code> and <code>ages[i]</code> represents the score and age of the <code>i<sup>th</sup></code> player, respectively, return <em>the highest overall score of all possible basketball teams</em>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> scores = [1,3,5,10,15], ages = [1,2,3,4,5]
<strong>Output:</strong> 34
<strong>Explanation:</strong>&nbsp;You can choose all the players.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> scores = [4,5,6,5], ages = [2,1,2,1]
<strong>Output:</strong> 16
<strong>Explanation:</strong>&nbsp;It is best to choose the last 3 players. Notice that you are allowed to choose multiple people of the same age.
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> scores = [1,2,3,5], ages = [8,9,10,1]
<strong>Output:</strong> 6
<strong>Explanation:</strong>&nbsp;It is best to choose the first 3 players. 
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= scores.length, ages.length &lt;= 1000</code></li>
	<li><code>scores.length == ages.length</code></li>
	<li><code>1 &lt;= scores[i] &lt;= 10<sup>6</sup></code></li>
	<li><code>1 &lt;= ages[i] &lt;= 1000</code></li>
</ul>


**Companies**:  
[Uber](https://leetcode.com/company/uber)

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/), [Sorting](https://leetcode.com/tag/sorting/)

## Solution 1. Recursion + Memoization

```cpp
// OJ: https://leetcode.com/problems/best-team-with-no-conflicts/
// Author: A M A N
// Time : O(N * M + N*logN)
// Space: O(N * M)
class Solution {
public:
    int dp[1001][10001];
    int solve(vector<pair<int, int>>&v, int idx, int mx){
        if(idx>=v.size()) return 0;
        if(dp[idx][mx]!=-1)
            return dp[idx][mx];
        //skip
        int A = solve(v, idx+1, mx);
        //pick if you can
        int B = 0;
        if(v[idx].second>=mx)
            B = v[idx].first + solve(v, idx+1, v[idx].second);
        return dp[idx][mx] = max(A, B);
    }
    int bestTeamScore(vector<int>& scores, vector<int>& ages) {
        vector<pair<int, int>> v;
        memset(dp, -1, sizeof dp);
        for(int i=0; i<scores.size(); i++)
            v.push_back({scores[i], ages[i]});
        sort(v.begin(), v.end());
        return solve(v, 0, 0);
    }
};
```