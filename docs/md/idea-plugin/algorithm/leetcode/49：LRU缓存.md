## 题目描述 
>  请你设计并实现一个满足  [LRU (最近最少使用) 缓存](https://baike.baidu.com/item/LRU) 约束的数据结构。
>
>  实现 `LRUCache` 类：
>
>  - `LRUCache(int capacity)` 以 **正整数** 作为容量 `capacity` 初始化 LRU 缓存
>  - `int get(int key)` 如果关键字 `key` 存在于缓存中，则返回关键字的值，否则返回 `-1` 。
>  - `void put(int key, int value)` 如果关键字 `key` 已经存在，则变更其数据值 `value` ；如果不存在，则向缓存中插入该组 `key-value` 。如果插入操作导致关键字数量超过 `capacity` ，则应该 **逐出** 最久未使用的关键字。
>
>  函数 `get` 和 `put` 必须以 `O(1)` 的平均时间复杂度运行。
>
>   
>
>  **示例：**
>
>  ```
>  输入
>  ["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
>  [[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
>  输出
>  [null, null, null, 1, null, -1, null, -1, 3, 4]
>  
>  解释
>  LRUCache lRUCache = new LRUCache(2);
>  lRUCache.put(1, 1); // 缓存是 {1=1}
>  lRUCache.put(2, 2); // 缓存是 {1=1, 2=2}
>  lRUCache.get(1);    // 返回 1
>  lRUCache.put(3, 3); // 该操作会使得关键字 2 作废，缓存是 {1=1, 3=3}
>  lRUCache.get(2);    // 返回 -1 (未找到)
>  lRUCache.put(4, 4); // 该操作会使得关键字 1 作废，缓存是 {4=4, 3=3}
>  lRUCache.get(1);    // 返回 -1 (未找到)
>  lRUCache.get(3);    // 返回 3
>  lRUCache.get(4);    // 返回 4
>  ```


## 方法一（有序哈希）
#### 1.解题思路
定义一个LinkedHashMap，对于get操作，如果不存在key，直接返回-1。如果存在，则删除掉原来的key，重新将对应的键值对添加进去。对于put操作，如果不存在key，要看当前map大小是否等于capacity，如果已经到了capacity大小，则删除掉有序哈希最开始的key；如果存在key，则直接删掉key。最后将键值对put到有序哈希。

#### 2.代码实现
```java
class LRUCache {
    int capacity;
    LinkedHashMap<Integer,Integer> map;
    public LRUCache(int capacity) {
        this.capacity = capacity;
        this.map = new LinkedHashMap<>();
    }
    
    public int get(int key) {
        if(!map.containsKey(key)){
            return -1;
        }
        int value = map.remove(key);
        map.put(key,value);
        return value;
    }
    
    public void put(int key, int value) {
        if(!map.containsKey(key)){
            if(map.size() == capacity){
                int deleteKey = map.keySet().iterator().next();
                map.remove(deleteKey);
            }
        }
        else{
            map.remove(key);
        }
        map.put(key,value);
    }
}
```
#### 3.复杂度分析

- 时间复杂度：与LinkedHashMap的get、put时间复杂度相同，所以平均时间复杂度为O(1)。

- 空间复杂度：需要额外大小为n的哈希表，所以空间复杂度为O(n) 。

