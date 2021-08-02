# [875. Koko Eating Bananas (Medium)](https://leetcode.com/problems/koko-eating-bananas/)

<p>Koko loves to eat bananas. There are <code>n</code> piles of bananas, the <code>i<sup>th</sup></code> pile has <code>piles[i]</code> bananas. The guards have gone and will come back in <code>h</code> hours.</p>

<p>Koko can decide her bananas-per-hour eating speed of <code>k</code>. Each hour, she chooses some pile of bananas and eats <code>k</code> bananas from that pile. If the pile has less than <code>k</code> bananas, she eats all of them instead and will not eat any more bananas during this hour.</p>

<p>Koko likes to eat slowly but still wants to finish eating all the bananas before the guards return.</p>

<p>Return <em>the minimum integer</em> <code>k</code> <em>such that she can eat all the bananas within</em> <code>h</code> <em>hours</em>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> piles = [3,6,7,11], h = 8
<strong>Output:</strong> 4
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> piles = [30,11,23,4,20], h = 5
<strong>Output:</strong> 30
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> piles = [30,11,23,4,20], h = 6
<strong>Output:</strong> 23
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= piles.length &lt;= 10<sup>4</sup></code></li>
	<li><code>piles.length &lt;= h &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= piles[i] &lt;= 10<sup>9</sup></code></li>
</ul>


**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Binary Search](https://leetcode.com/tag/binary-search/)

**Similar Questions**:
* [Minimize Max Distance to Gas Station (Hard)](https://leetcode.com/problems/minimize-max-distance-to-gas-station/)

## Solution 1. Binary Search on answers

```cpp
// OJ: https://leetcode.com/problems/koko-eating-bananas/
// Author: A M A N
// Time : O(N*log(S)) where S is the max element in the piles
// Space: O(1)
class Solution {
public:
    bool isValid(vector<int>&piles, int h, int speed){
        int hoursTaken = 0;
        for(int i=0; i<piles.size(); i++){
            hoursTaken += (piles[i]+speed-1)/speed; //ceil(piles[i]/speed)
        }
        return hoursTaken<=h;
    }
    int minEatingSpeed(vector<int>& piles, int h) {
        int ans = -1;
        int L = 1;
        int R = *max_element(piles.begin(), piles.end());
        while(L<=R){
            int M = L + (R-L)/2;
            if(isValid(piles, h, M)){
                ans = M;
                R = M-1;
            }else
                L = M+1;
        }
        return ans;
    }
};
```