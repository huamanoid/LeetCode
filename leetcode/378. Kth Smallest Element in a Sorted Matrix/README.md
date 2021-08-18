# [378. Kth Smallest Element in a Sorted Matrix (Medium)](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/)

<p>Given an <code>n x n</code> <code>matrix</code> where each of the rows and columns are sorted in ascending order, return <em>the</em> <code>k<sup>th</sup></code> <em>smallest element in the matrix</em>.</p>

<p>Note that it is the <code>k<sup>th</sup></code> smallest element <strong>in the sorted order</strong>, not the <code>k<sup>th</sup></code> <strong>distinct</strong> element.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> matrix = [[1,5,9],[10,11,13],[12,13,15]], k = 8
<strong>Output:</strong> 13
<strong>Explanation:</strong> The elements in the matrix are [1,5,9,10,11,12,13,<u><strong>13</strong></u>,15], and the 8<sup>th</sup> smallest number is 13
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> matrix = [[-5]], k = 1
<strong>Output:</strong> -5
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>n == matrix.length</code></li>
	<li><code>n == matrix[i].length</code></li>
	<li><code>1 &lt;= n &lt;= 300</code></li>
	<li><code>-10<sup>9</sup> &lt;= matrix[i][j] &lt;= 10<sup>9</sup></code></li>
	<li>All the rows and columns of <code>matrix</code> are <strong>guaranteed</strong> to be sorted in <strong>non-decreasing order</strong>.</li>
	<li><code>1 &lt;= k &lt;= n<sup>2</sup></code></li>
</ul>


**Companies**:  
[Facebook](https://leetcode.com/company/facebook), [Amazon](https://leetcode.com/company/amazon), [Apple](https://leetcode.com/company/apple)

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Binary Search](https://leetcode.com/tag/binary-search/), [Sorting](https://leetcode.com/tag/sorting/), [Heap (Priority Queue)](https://leetcode.com/tag/heap-priority-queue/), [Matrix](https://leetcode.com/tag/matrix/)

**Similar Questions**:
* [Find K Pairs with Smallest Sums (Medium)](https://leetcode.com/problems/find-k-pairs-with-smallest-sums/)
* [Kth Smallest Number in Multiplication Table (Hard)](https://leetcode.com/problems/kth-smallest-number-in-multiplication-table/)
* [Find K-th Smallest Pair Distance (Hard)](https://leetcode.com/problems/find-k-th-smallest-pair-distance/)
* [K-th Smallest Prime Fraction (Hard)](https://leetcode.com/problems/k-th-smallest-prime-fraction/)

## Solution 1. Heap

```cpp
// OJ: https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/
// Author: A M A N
// Time : O(N^2 * logK)
// Space: O(K)
class Solution {
public:
    int kthSmallest(vector<vector<int>>& matrix, int k) {
        priority_queue<int> pq; //max heap
        for(int i=0; i<matrix.size(); i++)
            for(int j=0; j<matrix[0].size(); j++){
                pq.push(matrix[i][j]);
                if(pq.size()>k){ // maintaining the size of heap at k for log(k) operations and 
                    pq.pop(); // to place our desired value at top
                }
            }
        return pq.top();
    }
};
```

## Solution 2. Heap

```cpp
// OJ: https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/
// Author: A M A N
// Time : O(KlogN)
// Space: O(N)
class Solution {
    typedef tuple<int, int, int> item;
public:
    int kthSmallest(vector<vector<int>>& A, int k) {
        int n = A.size();
        int dirs[2][2] = {{0,1},{1,0}}; // down, right only
        vector<vector<bool>>seen(n, vector<bool>(n, false));
        priority_queue<item, vector<item>, greater<>> pq; // min heap
        pq.push({A[0][0], 0, 0});
        seen[0][0] = true;
        while(--k){ // popping out k-1 smallest elements starting from origin
            auto [val, x, y] = pq.top(); pq.pop();
            for(auto&[dx, dy]: dirs){
                int X = x + dx;
                int Y = y + dy;
                if(X<0 or X>=n or Y<0 or Y>=n or seen[X][Y]) continue;
                seen[X][Y] = true;
                pq.push({A[X][Y], X, Y});
            }
        }
        return get<0>(pq.top()); // k th smallest element remains at the top
    }
};
```

## Solution 3. Binary Search

```cpp
// OJ: https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/
// Author: A M A N
// Time : O(Nlog(max(matrix)))
// Space: O(1)
class Solution {
public:
    int rank(vector<vector<int>>&matrix, int num){
        int n = matrix.size();
        int j = n-1, cnt=0;
        // if matrix[i][j] > num then matrix[i+1][j] will also be greater than num 
        // because matrix is sorted wrt to column as well
        for(int i=0; i < n; i++){
            while(j>=0 and matrix[i][j] > num) j--;
            cnt+= j + 1; // num is greater than or equal to j+1 elements in this row
        }
        return cnt;
    }
    int kthSmallest(vector<vector<int>>& matrix, int k) {
        int n = matrix.size();
        int L = matrix[0][0], R = matrix[n-1][n-1];
        while(L<R){
            int M = L + (R-L)/2;
            if(rank(matrix, M) < k) // rank returns the number of values less than M 
                L = M+1;
            else
                R = M;
        }
        return L;
    }
};
```