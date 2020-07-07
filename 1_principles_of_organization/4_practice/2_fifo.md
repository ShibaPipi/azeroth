## 实践 FIFO 缓存置换算法

>### FIFO 先进先出算法
* [FIFO 复习传送门](../2_organization/5_cache.md#高速缓存的替换策略)
* 淘汰缓存时，把最先进入链表的节点淘汰
* 具体实现（Python）
    
    ```python
        #! -*- encoding-utf-8 -*-
    
        from DoublyLinkedList import DoublyLinkedList, Node
  
        class FIFOCache(object):

            def __init__(self, capacity):
                self.capacity = capacity
                self.size = 0
                self.map = {}
                self.list = DoublyLinkedList(self.capacity)
  
            def get(self, key):
                if key not in self.map:
                    return -1
                else:
                    node = self.map.get(key)
                    return node.value
  
            def put(self, key, value):
                if 0 == self.capacity:
                    return
                if key in self.map:
                    node = self.map.get(key)
                    self.list.remove(node)
                    node.value = value
                    self.list.push(node)
                else:
                    if self.size == self.capacity:
                        node = self.list.pop()
                        del self.map[node.key]
                        self.size -= 1
                    node = Node(key, value)
                    self.list.push(node)
                    self.map[key] = node
                    self.size += 1
  
            def print(self):
                self.list.print()
    ```
