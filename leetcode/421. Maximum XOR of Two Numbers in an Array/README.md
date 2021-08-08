# [421. Maximum XOR of Two Numbers in an Array (Medium)](https://leetcode.com/problems/maximum-xor-of-two-numbers-in-an-array/)

<p>Given an integer array <code>nums</code>, return <em>the maximum result of </em><code>nums[i] XOR nums[j]</code>, where <code>0 &lt;= i &lt;= j &lt; n</code>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> nums = [3,10,5,25,2,8]
<strong>Output:</strong> 28
<strong>Explanation:</strong> The maximum result is 5 XOR 25 = 28.</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> nums = [0]
<strong>Output:</strong> 0
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> nums = [2,4]
<strong>Output:</strong> 6
</pre>

<p><strong>Example 4:</strong></p>

<pre><strong>Input:</strong> nums = [8,10,2]
<strong>Output:</strong> 10
</pre>

<p><strong>Example 5:</strong></p>

<pre><strong>Input:</strong> nums = [14,70,53,83,49,91,36,80,92,51,66,70]
<strong>Output:</strong> 127
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 2 * 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 2<sup>31</sup> - 1</code></li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Hash Table](https://leetcode.com/tag/hash-table/), [Bit Manipulation](https://leetcode.com/tag/bit-manipulation/), [Trie](https://leetcode.com/tag/trie/)

**Similar Questions**:
* [Maximum XOR With an Element From Array (Hard)](https://leetcode.com/problems/maximum-xor-with-an-element-from-array/)

## Solution 1. Trie

```cpp
// OJ: https://leetcode.com/problems/maximum-xor-of-two-numbers-in-an-array/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    struct TrieNode {
        TrieNode* left;
        TrieNode* right;
};
    void insert(TrieNode* head, int num){
        TrieNode* curr = head;
        for(int i=31;i>=0;i--){
            int bit = (num>>i)&1;
            if(bit==0){
                if(!curr->left)
                    curr->left = new TrieNode();
                curr = curr->left;
            }else {
                if(!curr->right)
                    curr->right = new TrieNode();
                curr = curr->right;
            }
        }
    }
    int findMaximumXOR(vector<int>& A) {
        TrieNode* head = new TrieNode();
        for(int a:A)
            insert(head, a);
        int ans = INT_MIN;
        for(int n:A){
            int temp = 0;
            TrieNode* curr = head;
            for(int i =31;i>=0;i--){
                int bit = (n>>i)&1;
                if(bit==0){
                    if(curr->right){
                        temp += (1<<i);
                        curr = curr->right;
                    }
                    else{
                        curr = curr->left;
                    }
                }else{
                    if(curr->left){
                        temp += (1<<i);
                        curr = curr->left;
                    }
                    else
                        curr = curr->right;
                }
            }
            ans = max(ans, temp);
        }
        return ans;
    }
};
```