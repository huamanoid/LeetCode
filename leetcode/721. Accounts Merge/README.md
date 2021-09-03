# [721. Accounts Merge (Medium)](https://leetcode.com/problems/accounts-merge/)

<p>Given a list of <code>accounts</code> where each element <code>accounts[i]</code> is a list of strings, where the first element <code>accounts[i][0]</code> is a name, and the rest of the elements are <strong>emails</strong> representing emails of the account.</p>

<p>Now, we would like to merge these accounts. Two accounts definitely belong to the same person if there is some common email to both accounts. Note that even if two accounts have the same name, they may belong to different people as people could have the same name. A person can have any number of accounts initially, but all of their accounts definitely have the same name.</p>

<p>After merging the accounts, return the accounts in the following format: the first element of each account is the name, and the rest of the elements are emails <strong>in sorted order</strong>. The accounts themselves can be returned in <strong>any order</strong>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> accounts = [["John","johnsmith@mail.com","john_newyork@mail.com"],["John","johnsmith@mail.com","john00@mail.com"],["Mary","mary@mail.com"],["John","johnnybravo@mail.com"]]
<strong>Output:</strong> [["John","john00@mail.com","john_newyork@mail.com","johnsmith@mail.com"],["Mary","mary@mail.com"],["John","johnnybravo@mail.com"]]
<strong>Explanation:</strong>
The first and third John's are the same person as they have the common email "johnsmith@mail.com".
The second John and Mary are different people as none of their email addresses are used by other accounts.
We could return these lists in any order, for example the answer [['Mary', 'mary@mail.com'], ['John', 'johnnybravo@mail.com'], 
['John', 'john00@mail.com', 'john_newyork@mail.com', 'johnsmith@mail.com']] would still be accepted.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> accounts = [["Gabe","Gabe0@m.co","Gabe3@m.co","Gabe1@m.co"],["Kevin","Kevin3@m.co","Kevin5@m.co","Kevin0@m.co"],["Ethan","Ethan5@m.co","Ethan4@m.co","Ethan0@m.co"],["Hanzo","Hanzo3@m.co","Hanzo1@m.co","Hanzo0@m.co"],["Fern","Fern5@m.co","Fern1@m.co","Fern0@m.co"]]
<strong>Output:</strong> [["Ethan","Ethan0@m.co","Ethan4@m.co","Ethan5@m.co"],["Gabe","Gabe0@m.co","Gabe1@m.co","Gabe3@m.co"],["Hanzo","Hanzo0@m.co","Hanzo1@m.co","Hanzo3@m.co"],["Kevin","Kevin0@m.co","Kevin3@m.co","Kevin5@m.co"],["Fern","Fern0@m.co","Fern1@m.co","Fern5@m.co"]]
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= accounts.length &lt;= 1000</code></li>
	<li><code>2 &lt;= accounts[i].length &lt;= 10</code></li>
	<li><code>1 &lt;= accounts[i][j] &lt;= 30</code></li>
	<li><code>accounts[i][0]</code> consists of English letters.</li>
	<li><code>accounts[i][j] (for j &gt; 0)</code> is a valid email.</li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [String](https://leetcode.com/tag/string/), [Depth-First Search](https://leetcode.com/tag/depth-first-search/), [Breadth-First Search](https://leetcode.com/tag/breadth-first-search/), [Union Find](https://leetcode.com/tag/union-find/)

**Similar Questions**:
* [Redundant Connection (Medium)](https://leetcode.com/problems/redundant-connection/)
* [Sentence Similarity (Easy)](https://leetcode.com/problems/sentence-similarity/)
* [Sentence Similarity II (Medium)](https://leetcode.com/problems/sentence-similarity-ii/)

## Solution 1. DSU

```cpp
// OJ: https://leetcode.com/problems/accounts-merge/
// Author: A M A N
// Time : O()
// Space: O()
class Solution {
public:
    unordered_map<string, string> parent;
    // O(N^2)
    // without path compression 
    // string find(string u){
    //     while(parent[u]!=u){
    //         u = parent[u];
    //     }
    //     return u;
    // }
    // O(N * log N)
    // with half path compression
    string find(string u){
        while(parent[u]!=u){
            parent[u] = parent[parent[u]]; // make every other node point to its parent node (compressing the path by half)
            //we can do better with recursion
            u = parent[u];
        }
        return u;
    }
    // O(N * log N) we can further decrease it to O(N) by uniting by Rank, 
    //i.e. always pointing smaller tree root to the bigger tree's root with the help of size array 
    // with path compression
    string find(string u){
        if(parent[u]!=u)
            parent[u] = find(parent[u]); // compresses the path for each node in between to the top most node
        return parent[u];
    }
    vector<vector<string>> accountsMerge(vector<vector<string>>& accounts) {
        unordered_map<string, string> owner;
        for(int i=0; i<accounts.size(); i++){
            for(int j=1; j<accounts[i].size(); j++){
                parent[accounts[i][j]] = accounts[i][j]; //initially self parent
                owner[accounts[i][j]]  = accounts[i][0]; // owner of the emails
            }
        }
        //Tricky Part
        for(int i=0; i<accounts.size(); i++){
            string p = find(accounts[i][1]);
            for(int j=2; j<accounts[i].size(); j++){
                parent[find(accounts[i][j])] = p; // making **first element's parents** the parent of all elements
            }
        }
        unordered_map<string, set<string>> unions;
        //save the emails with same parent to same set
        for(int i=0; i<accounts.size(); i++){
            for(int j=1; j<accounts[i].size(); j++){
                unions[find(accounts[i][j])].insert(accounts[i][j]);
            }
        }
        vector<vector<string>> ans;
        for(auto &p: unions){ //using call by reference helps get rid of copying each p 
            vector<string> emails(p.second.begin(), p.second.end());
            emails.insert(emails.begin(), owner[p.first]); // add owner name
            ans.push_back(emails);
        }
        return ans;
    }
};
```