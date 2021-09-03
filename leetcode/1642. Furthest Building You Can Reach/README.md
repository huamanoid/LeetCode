# [1642. Furthest Building You Can Reach (Medium)](https://leetcode.com/problems/furthest-building-you-can-reach/)

<p>You are given an integer array <code>heights</code> representing the heights of buildings, some <code>bricks</code>, and some <code>ladders</code>.</p>

<p>You start your journey from building <code>0</code> and move to the next building by possibly using bricks or ladders.</p>

<p>While moving from building <code>i</code> to building <code>i+1</code> (<strong>0-indexed</strong>),</p>

<ul>
	<li>If the current building's height is <strong>greater than or equal</strong> to the next building's height, you do <strong>not</strong> need a ladder or bricks.</li>
	<li>If the current building's height is <b>less than</b> the next building's height, you can either use <strong>one ladder</strong> or <code>(h[i+1] - h[i])</code> <strong>bricks</strong>.</li>
</ul>

<p><em>Return the furthest building index (0-indexed) you can reach if you use the given ladders and bricks optimally.</em></p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>
<img alt="" src="https://assets.leetcode.com/uploads/2020/10/27/q4.gif" style="width: 562px; height: 561px;">
<pre><strong>Input:</strong> heights = [4,2,7,6,9,14,12], bricks = 5, ladders = 1
<strong>Output:</strong> 4
<strong>Explanation:</strong> Starting at building 0, you can follow these steps:
- Go to building 1 without using ladders nor bricks since 4 &gt;= 2.
- Go to building 2 using 5 bricks. You must use either bricks or ladders because 2 &lt; 7.
- Go to building 3 without using ladders nor bricks since 7 &gt;= 6.
- Go to building 4 using your only ladder. You must use either bricks or ladders because 6 &lt; 9.
It is impossible to go beyond building 4 because you do not have any more bricks or ladders.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> heights = [4,12,2,7,3,18,20,3,19], bricks = 10, ladders = 2
<strong>Output:</strong> 7
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> heights = [14,3,19,3], bricks = 17, ladders = 0
<strong>Output:</strong> 3
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= heights.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= heights[i] &lt;= 10<sup>6</sup></code></li>
	<li><code>0 &lt;= bricks &lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt;= ladders &lt;= heights.length</code></li>
</ul>


**Companies**:  
[Google](https://leetcode.com/company/google), [Bloomberg](https://leetcode.com/company/bloomberg)

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Greedy](https://leetcode.com/tag/greedy/), [Heap (Priority Queue)](https://leetcode.com/tag/heap-priority-queue/)

## Solution 1. Greedy with MinHeap

```cpp
// OJ: https://leetcode.com/problems/furthest-building-you-can-reach/
// Author: A M A N
// Time : O(NlogL)
// Space: O(L) where L is the number of ladders
class Solution {
public:
    int furthestBuilding(vector<int>& heights, int bricks, int ladders) {
        priority_queue<int, vector<int>, greater<int>> pq; //min heap
        for(int i=0; i<heights.size()-1; i++){
            int hDiff = heights[i+1] - heights[i];
            if(hDiff<=0) // climbing down
                continue;
            if(ladders>0){ // use all the ladders while you have it
                pq.push(hDiff);
                ladders--;
            }else{
                // if the ladder used previously for a height diff that is smaller than current hDiff,
                // then replace that ladder with brick (if you have sufficient) and use that ladder here
                if(!pq.empty() and pq.top()<hDiff and bricks>=pq.top()){ 
                    bricks-=pq.top();
                    pq.pop();
                    pq.push(hDiff);
                }else if(bricks>=hDiff){ // else use bricks to climb the hDiff
                    bricks-=hDiff;
                }else { // return if you don't have bricks or ladders left
                    return i;
                }
            }
        }
        return heights.size()-1; // reached all the way to the end
    }
};
```

Greedy with MaxHeap

```cpp
// OJ: https://leetcode.com/problems/furthest-building-you-can-reach/
// Author: A M A N
// Time : O(NlogN)
// Space: O(N)
class Solution {
public:
    int furthestBuilding(vector<int>& heights, int bricks, int ladders) {
        priority_queue<int> pq; // max heap
        for(int i=0; i<heights.size()-1; i++){
            int hDiff = heights[i+1] - heights[i];
            if(hDiff<=0) continue;
            // Use bricks while you have it
            if(bricks>=hDiff){
                bricks-=hDiff;
                pq.push(hDiff); 
            } // if the bricks used previously for a height diff that is greater than current hDiff + you have ladders,
                // then replace those bricks with a ladder from previous position and keep bricks at current position + add the current hDiff to the heap
            else if(!pq.empty() and pq.top()>hDiff and ladders){
                bricks+=pq.top(); // taking back bricks from prev place 
                bricks-=hDiff; // using bricks at current position
                pq.pop();
                pq.push(hDiff); // enqueueing the current hDiff 
                ladders--; // using ladder at the prev place
            } else if(ladders) // else just use ladder at current position
                ladders--;
            else // nothing amenities left
                return i; 
        }
        return heights.size()-1;
    }
};
```

## Solution 2. Binary Search on Answer space



```cpp
// OJ: https://leetcode.com/problems/furthest-building-you-can-reach/
// Author: A M A N
// Time : O(N*logN*logN)
// Space: O(N)
class Solution {
public:
    bool isReachable(vector<int>&heights, int end, int bricks, int ladders){
        priority_queue<int, vector<int>, greater<int>> pq; //min heap (here, no advantage over sorting)
        for(int i=0; i<end; i++){ // i+1 will go upto `end` which means end building is reachable
            int hDiff = heights[i+1] - heights[i]; 
            if(hDiff>0)
                pq.push(hDiff);
        }
        while(!pq.empty()){
            if(bricks>=pq.top())
                bricks-=pq.top();
            else if(ladders)
                ladders--;
            else 
                return false;
            pq.pop();
        }
        return true;        
    }
    int furthestBuilding(vector<int>& heights, int bricks, int ladders) {
        int L = 0, R = heights.size()-1;
        while(L<R){
            int M = (L+R+1)/2;
            if(isReachable(heights, M, bricks, ladders)){
                L = M;
            } else
                R = M-1;
        }
        return L;
    }
};
```

> Why M = (L+R+1)/2 ?

On an odd-lengthed search space, identifying the midpoint is straightforward. On `even-lengthed search spaces`, though, we have `two possible midpoints`. The final step of the binary search algorithm design process is to `decide` whether it is the `lower or higher midpoint` that should be used.

`Your decision should be based on how you are updating hi and lo` (i.e., lo = mid and hi = mid - 1 for the algorithm we've designed here). Think about what happens once the search space is of length-two. `You must ensure that the search space is guaranteed to be shrunk down to length-one, regardless of which condition is executed.` If you take the lower middle, it will sometimes infinitely loop. And if you take the upper middle, it will be guaranteed to shrink the search space down to one.

So, it is the upper-middle that we want.

Whenever we want the `upper middle`, we use either `mid = (lo + hi + 1) / 2` or mid = lo + (hi - lo + 1) / 2. These formulas ensure that on even-lengthed search spaces, the upper middle is chosen and on odd-lengthed search spaces, the actual middle is chosen.

> The short rule to remember is: if you used hi = mid - 1, then use the higher midpoint. If you used lo = mid + 1, then use the lower midpoint. If you used both of these, then you can use either midpoint. If you didn’t use either (i.e., you have lo = mid and hi = mid), then, unfortunately, your code is buggy, and you won’t be able to guarantee convergence.
