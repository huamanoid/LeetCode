# [957. Prison Cells After N Days (Medium)](https://leetcode.com/problems/prison-cells-after-n-days/)

<p>There are <code>8</code> prison cells in a row and each cell is either occupied or vacant.</p>

<p>Each day, whether the cell is occupied or vacant changes according to the following rules:</p>

<ul>
	<li>If a cell has two adjacent neighbors that are both occupied or both vacant, then the cell becomes occupied.</li>
	<li>Otherwise, it becomes vacant.</li>
</ul>

<p><strong>Note</strong> that because the prison is a row, the first and the last cells in the row can't have two adjacent neighbors.</p>

<p>You are given an integer array <code>cells</code> where <code>cells[i] == 1</code> if the <code>i<sup>th</sup></code> cell is occupied and <code>cells[i] == 0</code> if the <code>i<sup>th</sup></code> cell is vacant, and you are given an integer <code>n</code>.</p>

<p>Return the state of the prison after <code>n</code> days (i.e., <code>n</code> such changes described above).</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> cells = [0,1,0,1,1,0,0,1], n = 7
<strong>Output:</strong> [0,0,1,1,0,0,0,0]
<strong>Explanation:</strong> The following table summarizes the state of the prison on each day:
Day 0: [0, 1, 0, 1, 1, 0, 0, 1]
Day 1: [0, 1, 1, 0, 0, 0, 0, 0]
Day 2: [0, 0, 0, 0, 1, 1, 1, 0]
Day 3: [0, 1, 1, 0, 0, 1, 0, 0]
Day 4: [0, 0, 0, 0, 0, 1, 0, 0]
Day 5: [0, 1, 1, 1, 0, 1, 0, 0]
Day 6: [0, 0, 1, 0, 1, 1, 0, 0]
Day 7: [0, 0, 1, 1, 0, 0, 0, 0]
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> cells = [1,0,0,1,0,0,1,0], n = 1000000000
<strong>Output:</strong> [0,0,1,1,1,1,1,0]
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>cells.length == 8</code></li>
	<li><code>cells[i]</code>&nbsp;is either <code>0</code> or <code>1</code>.</li>
	<li><code>1 &lt;= n &lt;= 10<sup>9</sup></code></li>
</ul>


**Companies**:  
[Amazon](https://leetcode.com/company/amazon)

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Hash Table](https://leetcode.com/tag/hash-table/), [Math](https://leetcode.com/tag/math/), [Bit Manipulation](https://leetcode.com/tag/bit-manipulation/)

## Solution 1. 


The range is quite big to even adjust a linear solution suggesting some trickery.
At max there could be 256 states, and it should repeat after a certain period of mutation.


```cpp
// OJ: https://leetcode.com/problems/prison-cells-after-n-days/
// Author: A M A N
// Time : O(1) at max 256 states
// Space: O(1)
class Solution {
    vector<int> mutate(vector<int>& v){
        vector<int> res = {0};
        for(int i=1; i<v.size()-1; i++)
            res.push_back(v[i-1]==v[i+1]);
        res.push_back(0);
        return res;
    }
public:
    map<vector<int>, int> pos;
    unordered_map<int, vector<int>> states;
    
    vector<int> solve(vector<int>&cells, int i, int n){
        if(i==n+1) return cells;
        if(pos.find(cells)!=pos.end()){
            int T = i - pos[cells];
            int offset = pos[cells];
            int idx = (n-i)%T + offset;
            return states[idx];
        }
        pos[cells] = i;
        auto newCells = states[i] = mutate(cells);
        return solve(newCells, i+1, n);
    }
    vector<int> prisonAfterNDays(vector<int>& cells, int n) {
        return solve(cells, 1, n);
    }
};
```

Implementing it without recursive call stack.

```cpp
// OJ: https://leetcode.com/problems/prison-cells-after-n-days/
// Author: A M A N
// Time : O(1)
// Space: O(1)
class Solution {
    vector<int> mutate(vector<int>& v){
        vector<int> res = {0};
        for(int i=1; i<v.size()-1; i++)
            res.push_back(v[i-1]==v[i+1]);
        res.push_back(0);
        return res;
    }
public:
    map<vector<int>, int> pos;
    unordered_map<int, vector<int>> states;

    vector<int> prisonAfterNDays(vector<int>& cells, int n) {
        for(int i=1; i<=n; i++){
            cells = states[i] = mutate(cells);
            if(pos.find(cells)!=pos.end()){
                int T = i - pos[cells]; // Time period of the cycle
                int offset = pos[cells]; // start of the cycle
                int idx = (n-i)%T + offset; // equivalent distance + start of the cycle
                return states[idx];
            }
            pos[cells] = i;
        }
        return cells;
    }
};
```

Using a custom function to hash a vector to use an unordered_map with vector as a key for O(1) lookup and insertion.

```cpp
// OJ: https://leetcode.com/problems/prison-cells-after-n-days/
// Author: A M A N
// Time : O(1)
// Space: O(1)
class Solution {
    vector<int> mutate(vector<int>& v){
        vector<int> res = {0};
        for(int i=1; i<v.size()-1; i++)
            res.push_back(v[i-1]==v[i+1]);
        res.push_back(0);
        return res;
    }
public:
    struct VectorHasher {
      size_t operator()(const vector<int>& V) const {
        size_t hash = V.size();
        for (auto i : V)
          hash ^= i + 0x9e3779b9 + (hash << 6) + (hash >> 2);
        return hash;
      }
    };

    unordered_map<vector<int>, int, VectorHasher> pos;
    unordered_map<int, vector<int>> states;
    vector<int> prisonAfterNDays(vector<int>& cells, int n) {
        for(int i=1; i<=n; i++){
            cells = states[i] = mutate(cells);
            if(pos.find(cells)!=pos.end()){
                int T = i - pos[cells]; // Time period of the cycle
                int offset = pos[cells]; // start of the cycle
                int idx = (n-i)%T + offset; // equivalent distance + start of the cycle
                return states[idx];
            }
            pos[cells] = i;
        }
        return cells;
    }
};
```
