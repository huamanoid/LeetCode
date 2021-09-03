# [146. LRU Cache (Medium)](https://leetcode.com/problems/lru-cache/)

<p>Design a data structure that follows the constraints of a <strong><a href="https://en.wikipedia.org/wiki/Cache_replacement_policies#LRU" target="_blank">Least Recently Used (LRU) cache</a></strong>.</p>

<p>Implement the <code>LRUCache</code> class:</p>

<ul>
	<li><code>LRUCache(int capacity)</code> Initialize the LRU cache with <strong>positive</strong> size <code>capacity</code>.</li>
	<li><code>int get(int key)</code> Return the value of the <code>key</code> if the key exists, otherwise return <code>-1</code>.</li>
	<li><code>void put(int key, int value)</code>&nbsp;Update the value of the <code>key</code> if the <code>key</code> exists. Otherwise, add the <code>key-value</code> pair to the cache. If the number of keys exceeds the <code>capacity</code> from this operation, <strong>evict</strong> the least recently used key.</li>
</ul>

<p>The functions&nbsp;<code data-stringify-type="code">get</code>&nbsp;and&nbsp;<code data-stringify-type="code">put</code>&nbsp;must each run in <code>O(1)</code> average time complexity.</p>

<p>&nbsp;</p>
<p><strong>Example 1:</strong></p>

<pre><strong>Input</strong>
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
<strong>Output</strong>
[null, null, null, 1, null, -1, null, -1, 3, 4]

<strong>Explanation</strong>
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 1); // cache is {1=1}
lRUCache.put(2, 2); // cache is {1=1, 2=2}
lRUCache.get(1);    // return 1
lRUCache.put(3, 3); // LRU key was 2, evicts key 2, cache is {1=1, 3=3}
lRUCache.get(2);    // returns -1 (not found)
lRUCache.put(4, 4); // LRU key was 1, evicts key 1, cache is {4=4, 3=3}
lRUCache.get(1);    // return -1 (not found)
lRUCache.get(3);    // return 3
lRUCache.get(4);    // return 4
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= capacity &lt;= 3000</code></li>
	<li><code>0 &lt;= key &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= value &lt;= 10<sup>5</sup></code></li>
	<li>At most 2<code>&nbsp;* 10<sup>5</sup></code>&nbsp;calls will be made to <code>get</code> and <code>put</code>.</li>
</ul>


**Companies**:  
[Amazon](https://leetcode.com/company/amazon), [Microsoft](https://leetcode.com/company/microsoft), [Facebook](https://leetcode.com/company/facebook), [Apple](https://leetcode.com/company/apple), [Intuit](https://leetcode.com/company/intuit), [Google](https://leetcode.com/company/google), [Bloomberg](https://leetcode.com/company/bloomberg), [Snapchat](https://leetcode.com/company/snapchat), [VMware](https://leetcode.com/company/vmware), [eBay](https://leetcode.com/company/ebay), [Paypal](https://leetcode.com/company/paypal), [Twilio](https://leetcode.com/company/twilio), [Uber](https://leetcode.com/company/uber), [Zillow](https://leetcode.com/company/zillow), [Dropbox](https://leetcode.com/company/dropbox), [Salesforce](https://leetcode.com/company/salesforce), [Adobe](https://leetcode.com/company/adobe), [Walmart Labs](https://leetcode.com/company/walmart-labs), [Shopee](https://leetcode.com/company/shopee), [Cisco](https://leetcode.com/company/cisco), [Tesla](https://leetcode.com/company/tesla), [ByteDance](https://leetcode.com/company/bytedance), [Oracle](https://leetcode.com/company/oracle), [Goldman Sachs](https://leetcode.com/company/goldman-sachs), [JPMorgan](https://leetcode.com/company/jpmorgan), [Intel](https://leetcode.com/company/intel), [HBO](https://leetcode.com/company/hbo), [Twitch](https://leetcode.com/company/twitch), [Cohesity](https://leetcode.com/company/cohesity), [Splunk](https://leetcode.com/company/splunk)

**Related Topics**:  
[Hash Table](https://leetcode.com/tag/hash-table/), [Linked List](https://leetcode.com/tag/linked-list/), [Design](https://leetcode.com/tag/design/), [Doubly-Linked List](https://leetcode.com/tag/doubly-linked-list/)

**Similar Questions**:
* [LFU Cache (Hard)](https://leetcode.com/problems/lfu-cache/)
* [Design In-Memory File System (Hard)](https://leetcode.com/problems/design-in-memory-file-system/)
* [Design Compressed String Iterator (Easy)](https://leetcode.com/problems/design-compressed-string-iterator/)
* [Design Most Recently Used Queue (Medium)](https://leetcode.com/problems/design-most-recently-used-queue/)

## Solution 1. Linked-list + Hashing

```cpp
// OJ: https://leetcode.com/problems/lru-cache/
// Author: A M A N
// Time : O(1)
// Space: O(N)
class LRUCache {
public:
    int N;
    list<pair<int, int>> data; // key, value pairs (Most Recently used ---> Least Recently used)
    unordered_map<int, list<pair<int, int>>::iterator> mp; // maps key to its address (corresponding iterator pointing into data)
    LRUCache(int capacity) {
        N = capacity;
    }
    // anytime we see a node we move it to front in O(1) using doubly linked list 
    void moveToFront(int key){ 
        auto it = mp[key];      //old address of the key value pair
        data.splice(data.begin(), data, it); // removes the node and inserts at the begining 
        mp[key] = data.begin(); //new address of the key value pair
    }
    int get(int key) {
        if(mp.find(key)!=mp.end()){
            moveToFront(key);
            return mp[key]->second;
        }
        return -1;
    }
    void put(int key, int value) {
        if(mp.find(key)!=mp.end()){
            moveToFront(key);
            mp[key]->second = value;
        }else{
            data.push_front({key, value});
            mp[key] = data.begin();
            if(data.size() > N){
                int LRU_Key = data.rbegin()->first; // the last element in the data list is the Least Recently Used element
                mp.erase(LRU_Key); // evict the LRU
                data.pop_back();   // maintain the size of data to the capacity
            }
        }
    }
};
/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache* obj = new LRUCache(capacity);
 * int param_1 = obj->get(key);
 * obj->put(key,value);
 */
```