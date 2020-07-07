## 实践 LRU 最不经常使用算法

>### LRU 最不经常使用算法
* [LFU 复习传送门](../2_organization/5_cache.md#高速缓存的替换策略)
* 记录每个缓存的使用频率，淘汰缓存时，把使用频率最小的淘汰
* 将相同时用频率的缓存归纳起来，使用频率相同时，按 FIFO 算法淘汰缓存
* 具体实现（Python）
    
    ```python
        #! -*- encoding-utf-8 -*-
    
        from DoublyLinkedList import DoublyLinkedList, Node
  
        class LFUNode(Node):
  
            def __init__(self, key, value):
                self.freq = 0
                super(LFUNode, self).__init__(key, value)
  
        class LFUCache(object):
  
            def __init__(self, capacity):
                self.capacity = capacity
                self.map = {}
                # key: 频率，value: 频率对应的双向链表
                self.freq_map = {}
                self.list = DoublyLinkedList(self.capacity)
                self.size = 0
            
            # 更新节点频率的操作
            def __update_freq(self, node):
                freq = node.freq
                # 删除
                node = self.freq_map[freq].remove(node)
                if 0 == self.freq_map[freq].size:
                    del self.freq_map[freq]
                
                # 更新
                freq += 1
                node.freq = freq
                if freq not in self.freq_map:
                    self.freq_map[freq].push(node)
                
            def get(self, key):
                if key not in self.map:
                    return -1
                node = self.map.get(key)
                self.__update_freq(node)
                return node.value
                
            def put(self, key, value):
                if 0 == self.capacity:
                    return
                # 缓存命中
                if key in self.map:
                    node = self.map.get(key)
                    node.value = value
                    self.__update_freq(node)
                # 缓存未命中
                else:
                    if self.vapacity == self.size:
                        min_freq = min(self.freq_map)
                        node = self.freq_map[min_freq].pop()
                        del self.map[node.key]
                        self.size -= 1
                    node = LFUNode(key, value)
                    node.freq = 1
                    self.map[key] = node
                    if node.freq not in self.freq_map:
                        self.freq_map[node.freq] = DoublyLinkedList()
                    node = self.freq_map[node.freq].push(node)
                    self.size += 1
            
            def print(self):
                print('****************')
                for k, v in self.freq_map.items():
                    print('Freq = %d' % k)
                    self.freq_map[k].print()
                print('****************')
                print()
    ```
