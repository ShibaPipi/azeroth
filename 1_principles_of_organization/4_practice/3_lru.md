## 实践 LRU 缓存置换算法

>### LRU 最近最少使用算法算法
* [LRU 复习传送门](../2_organization/5_cache.md#高速缓存的替换策略)
* 每次使用，把使用过的节点放到链表的最前面
* 具体实现（Python）
    
    ```python
        #! -*- encoding-utf-8 -*-
    
        from DoublyLinkedList import DoublyLinkedList, Node
  
        class LRUCache(object):
  
            def __init__(self, capacity):
                self.capacity = capacity
                self.map = {}
                self.list = DoublyLinkedList(self.capacity)
            
            def get(self, key):
                if key in self.map:
                    node = self.map[key]
                    self.list.remove(node)
                    self.list.unshift(node)
                else:
                    return -1
  
            def put(self, key, value):
                if key in self.map:
                    node = self.map.get(key)
                    self.list.remove(node)
                    node.value = value
                    self.list.unshift(node)
                else:
                    node = Node(key, value)
                    # 缓存已经满了
                    if self.list.size >= self.list.capacity:
                        old_node = self.list.remove()
                        self.map.pop(old_node.key)
                    self.list.unshift(node)
                    self.map[key] = node
  
            def print(self):
                self.list.print()
  ```
