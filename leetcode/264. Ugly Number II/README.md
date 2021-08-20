# [264. Ugly Number II (Medium)](https://leetcode.com/problems/ugly-number-ii/)

<p>An <strong>ugly number</strong> is a positive integer whose prime factors are limited to <code>2</code>, <code>3</code>, and <code>5</code>.</p>

<p>Given an integer <code>n</code>, return <em>the</em> <code>n<sup>th</sup></code> <em><strong>ugly number</strong></em>.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> n = 10
<strong>Output:</strong> 12
<strong>Explanation:</strong> [1, 2, 3, 4, 5, 6, 8, 9, 10, 12] is the sequence of the first 10 ugly numbers.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> n = 1
<strong>Output:</strong> 1
<strong>Explanation:</strong> 1 has no prime factors, therefore all of its prime factors are limited to 2, 3, and 5.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 1690</code></li>
</ul>


**Companies**:  
[Adobe](https://leetcode.com/company/adobe), [Amazon](https://leetcode.com/company/amazon), [Citadel](https://leetcode.com/company/citadel)

**Related Topics**:  
[Hash Table](https://leetcode.com/tag/hash-table/), [Math](https://leetcode.com/tag/math/), [Dynamic Programming](https://leetcode.com/tag/dynamic-programming/), [Heap (Priority Queue)](https://leetcode.com/tag/heap-priority-queue/)

**Similar Questions**:
* [Merge k Sorted Lists (Hard)](https://leetcode.com/problems/merge-k-sorted-lists/)
* [Count Primes (Easy)](https://leetcode.com/problems/count-primes/)
* [Ugly Number (Easy)](https://leetcode.com/problems/ugly-number/)
* [Perfect Squares (Medium)](https://leetcode.com/problems/perfect-squares/)
* [Super Ugly Number (Medium)](https://leetcode.com/problems/super-ugly-number/)
* [Ugly Number III (Medium)](https://leetcode.com/problems/ugly-number-iii/)

## Solution 1. Brute-force

```cpp
// OJ: https://leetcode.com/problems/ugly-number-ii/
// Author: A M A N
// Time : O()
// Space: O()
class Solution {
public:
    int nthUglyNumber(int n) {
        priority_queue<int> pq;
        for(int i=0; i<=31; i++){
            for(int j=0; j<=20; j++){
                for(int k=0; k<=14; k++){
                    long double num = 1LL* powl(2, i) * powl(3, j) * powl(5, k);
                    if(num>INT_MAX)break;
                    pq.push(num);
                    if(pq.size()>n)
                        pq.pop();
                }
            }
        }
        return pq.top();
    }
};
```

## Solution 2. Min Heap

```cpp
// OJ: https://leetcode.com/problems/ugly-number-ii/
// Author: A M A N
// Time : O(NlogN)
// Space: O(N)
class Solution {
public:
    int nthUglyNumber(int n) {
        int64_t cur;
        unordered_set<int64_t> sett;
        priority_queue<int64_t, vector<int64_t>, greater<int64_t>> pq; //min heap
        pq.push(1);
        int cnt = 0;
        while(cnt++<n){
            cur = pq.top();
            if(!sett.count(2*cur)){
                pq.push(2*cur);
                sett.insert(2*cur);
            }
            if(!sett.count(3*cur)){
                pq.push(3*cur);
                sett.insert(3*cur);
            }
            if(!sett.count(5*cur)){
                pq.push(5*cur);
                sett.insert(5*cur);
            }
            pq.pop();
        }
        return cur;
    }
};
```

## Solution 3. DP

```cpp
// OJ: https://leetcode.com/problems/ugly-number-ii/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    int nthUglyNumber(int n) {
        vector<int> v(n);
        v[0] = 1;
        int i=0, j=0, k=0; // i, j, k points to previously calculated ugly numbers
        for(int t=1; t<n; t++){
            v[t] = min({v[i]*2, v[j]*3, v[k]*5}); // the next ugly number must be the min of these
            if(v[t]==v[i]*2) i++;
            if(v[t]==v[j]*3) j++;
            if(v[t]==v[k]*5) k++;
        }
        return v[n-1];
    }
};

```