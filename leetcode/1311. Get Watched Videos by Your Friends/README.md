# [1311. Get Watched Videos by Your Friends (Medium)](https://leetcode.com/problems/get-watched-videos-by-your-friends/)

<p>There are <code>n</code> people, each person has a unique <em>id</em> between <code>0</code> and <code>n-1</code>. Given the arrays <code>watchedVideos</code> and <code>friends</code>, where <code>watchedVideos[i]</code> and <code>friends[i]</code> contain the list of watched videos and the list of friends respectively for the person with <code>id = i</code>.</p>

<p>Level <strong>1</strong> of videos are all watched videos by your&nbsp;friends, level <strong>2</strong> of videos are all watched videos by the friends of your&nbsp;friends and so on. In general, the level <code>k</code> of videos are all&nbsp;watched videos by people&nbsp;with the shortest path <strong>exactly</strong> equal&nbsp;to&nbsp;<code>k</code> with you. Given your&nbsp;<code>id</code> and the <code>level</code> of videos, return the list of videos ordered by their frequencies (increasing). For videos with the same frequency order them alphabetically from least to greatest.&nbsp;</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<p><strong><img alt="" src="https://assets.leetcode.com/uploads/2020/01/02/leetcode_friends_1.png" style="width: 144px; height: 200px;"></strong></p>

<pre><strong>Input:</strong> watchedVideos = [["A","B"],["C"],["B","C"],["D"]], friends = [[1,2],[0,3],[0,3],[1,2]], id = 0, level = 1
<strong>Output:</strong> ["B","C"] 
<strong>Explanation:</strong> 
You have id = 0 (green color in the figure) and your friends are (yellow color in the figure):
Person with id = 1 -&gt; watchedVideos = ["C"]&nbsp;
Person with id = 2 -&gt; watchedVideos = ["B","C"]&nbsp;
The frequencies of watchedVideos by your friends are:&nbsp;
B -&gt; 1&nbsp;
C -&gt; 2
</pre>

<p><strong>Example 2:</strong></p>

<p><strong><img alt="" src="https://assets.leetcode.com/uploads/2020/01/02/leetcode_friends_2.png" style="width: 144px; height: 200px;"></strong></p>

<pre><strong>Input:</strong> watchedVideos = [["A","B"],["C"],["B","C"],["D"]], friends = [[1,2],[0,3],[0,3],[1,2]], id = 0, level = 2
<strong>Output:</strong> ["D"]
<strong>Explanation:</strong> 
You have id = 0 (green color in the figure) and the only friend of your friends is the person with id = 3 (yellow color in the figure).
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>n == watchedVideos.length ==&nbsp;friends.length</code></li>
	<li><code>2 &lt;= n&nbsp;&lt;= 100</code></li>
	<li><code>1 &lt;=&nbsp;watchedVideos[i].length &lt;= 100</code></li>
	<li><code>1 &lt;=&nbsp;watchedVideos[i][j].length &lt;= 8</code></li>
	<li><code>0 &lt;= friends[i].length &lt; n</code></li>
	<li><code>0 &lt;= friends[i][j]&nbsp;&lt; n</code></li>
	<li><code>0 &lt;= id &lt; n</code></li>
	<li><code>1 &lt;= level &lt; n</code></li>
	<li>if&nbsp;<code>friends[i]</code> contains <code>j</code>, then <code>friends[j]</code> contains <code>i</code></li>
</ul>


**Companies**:  
[Amazon](https://leetcode.com/company/amazon)

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Hash Table](https://leetcode.com/tag/hash-table/), [Breadth-First Search](https://leetcode.com/tag/breadth-first-search/), [Sorting](https://leetcode.com/tag/sorting/)

## Solution 1. BFS + Hashing

```cpp
// OJ: https://leetcode.com/problems/get-watched-videos-by-your-friends/
// Author: A M A N
// Time : O(N + VlogV) where N is no of people and, V is the count of result videos 
// Space: O(N)
class Solution {
public:
    vector<string> watchedVideosByFriends(vector<vector<string>>& watchedVideos, vector<vector<int>>& friends, int id, int level) {
        int n = friends.size();
        // breadth first search for level K friends
        vector<bool> vis(n, false);
        queue<int> q;
        q.push(id);
        vis[id] = 1;
        int curLevel = 0;
        while(!q.empty()){
            for(int i=q.size(); i>0; i--){
                int u = q.front(); q.pop();
                for(auto v: friends[u]){ // friends is an adjacency list
                    if(vis[v])continue;
                    vis[v] = 1;
                    q.push(v);
                }
            }
            curLevel++;
            if(curLevel==level){
                break;
            }
        }
        // Now the queue has friends of level K
        map<string, int> mp; // string => freq
        while(q.size()){
            int u = q.front(); q.pop();
            for(auto videos: watchedVideos[u])
                mp[videos]++;
        }
        vector<string> ans;
        map<int, vector<string>> freqMap; // freq => string(s)
        for(auto a: mp)
            freqMap[a.second].push_back(a.first);
        for(auto a: freqMap)
            for(string s: a.second)
                ans.push_back(s);
        return ans;
    }
};
```
Using lambda in sorting instead of using another map.

```cpp
// OJ: https://leetcode.com/problems/get-watched-videos-by-your-friends/
// Author: A M A N
// Time : O(N + VlogV) where N is no of people and, V is the count of result videos 
// Space: O(N)
class Solution {
public:
    vector<string> watchedVideosByFriends(vector<vector<string>>& watchedVideos, vector<vector<int>>& friends, int id, int level) {
        int n = friends.size();
        // breadth first search for level K friends
        vector<bool> vis(n, false);
        queue<int> q;
        q.push(id);
        vis[id] = 1;
        int curLevel = 0;
        while(!q.empty()){
            for(int i=q.size(); i>0; i--){
                int u = q.front(); q.pop();
                for(auto v: friends[u]){ // friends is an adjacency list
                    if(vis[v])continue;
                    vis[v] = 1;
                    q.push(v);
                }
            }
            curLevel++;
            if(curLevel==level){
                break;
            }
        }
        // Now the queue has friends of level K
        unordered_map<string, int> mp; // string => freq
        while(q.size()){
            int u = q.front(); q.pop();
            for(auto videos: watchedVideos[u])
                mp[videos]++;
        }
        vector<string> ans;
        for(auto [s, freq]: mp) ans.push_back(s);
        sort(ans.begin(), ans.end(), [&](const string& a, const string& b){
            return mp[a]!=mp[b]?mp[a]<mp[b]:a<b;
        });
        return ans;
    }
};
```