# [735. Asteroid Collision (Medium)](https://leetcode.com/problems/asteroid-collision/)

<p>We are given an array <code>asteroids</code> of integers representing asteroids in a row.</p>

<p>For each asteroid, the absolute value represents its size, and the sign represents its direction (positive meaning right, negative meaning left). Each asteroid moves at the same speed.</p>

<p>Find out the state of the asteroids after all collisions. If two asteroids meet, the smaller one will explode. If both are the same size, both will explode. Two asteroids moving in the same direction will never meet.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input:</strong> asteroids = [5,10,-5]
<strong>Output:</strong> [5,10]
<b>Explanation:</b> The 10 and -5 collide resulting in 10. The 5 and 10 never collide.
</pre>

<p><strong>Example 2:</strong></p>

<pre><strong>Input:</strong> asteroids = [8,-8]
<strong>Output:</strong> []
<b>Explanation:</b> The 8 and -8 collide exploding each other.
</pre>

<p><strong>Example 3:</strong></p>

<pre><strong>Input:</strong> asteroids = [10,2,-5]
<strong>Output:</strong> [10]
<b>Explanation:</b> The 2 and -5 collide resulting in -5. The 10 and -5 collide resulting in 10.
</pre>

<p><strong>Example 4:</strong></p>

<pre><strong>Input:</strong> asteroids = [-2,-1,1,2]
<strong>Output:</strong> [-2,-1,1,2]
<b>Explanation:</b> The -2 and -1 are moving left, while the 1 and 2 are moving right. Asteroids moving the same direction never meet, so no asteroids will meet each other.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>2 &lt;= asteroids.length &lt;= 10<sup>4</sup></code></li>
	<li><code>-1000 &lt;= asteroids[i] &lt;= 1000</code></li>
	<li><code>asteroids[i] != 0</code></li>
</ul>


**Companies**:  
[Google](https://leetcode.com/company/google), [Amazon](https://leetcode.com/company/amazon), [Microsoft](https://leetcode.com/company/microsoft), [Lyft](https://leetcode.com/company/lyft), [ByteDance](https://leetcode.com/company/bytedance), [Facebook](https://leetcode.com/company/facebook)

**Related Topics**:  
[Array](https://leetcode.com/tag/array/), [Stack](https://leetcode.com/tag/stack/)

**Similar Questions**:
* [Can Place Flowers (Easy)](https://leetcode.com/problems/can-place-flowers/)

## Solution 1. Stack

```cpp
// OJ: https://leetcode.com/problems/asteroid-collision/
// Author: A M A N
// Time : O(N)
// Space: O(N)
class Solution {
public:
    vector<int> asteroidCollision(vector<int>& asteroids) {
        vector<int> st;
        for(int i=0; i<asteroids.size(); i++){
            int cur = asteroids[i];
            if(cur>0) st.push_back(cur);
            else if(!st.empty()){
                if(st.back()>0 and cur<0){ //collision happens only when (=> and <=) meets
                    bool destroyed = false;
                    while(st.size() and st.back()>0 and st.back() <= abs(cur)){
                        if(st.back()==abs(cur)) {st.pop_back(); destroyed = true; break;} //if equal in magnitude, destory both 
                        st.pop_back();
                    }
                    if(!destroyed)
                        //incoming: "<=" can only be added without any more collision if previous: is also "<=" or NONE
                        if(st.empty()) st.push_back(cur);
                        else if(st.back()<0) st.push_back(cur); 
                }
                else // non collision asteroids (=> and =>) or (<= and <=) or (<= and =>)
                    st.push_back(cur);
            } else {
                st.push_back(cur);
            }
        }
        return st;
    }
};
```