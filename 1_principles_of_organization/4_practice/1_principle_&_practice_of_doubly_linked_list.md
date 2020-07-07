## 双向链表的原理与实践

>### 单向链表和双向链表
* 单向链表
    * 每一个节点都有下一个节点的地址或引用
        * 节点 1 `->` 节点 2 `->` 节点 3 `->` 节点 4 `->` 节点 5
* 双向链表
    * 每一个节点都有上一个节点和下一个节点的地址或引用
        * 节点 1 `<->` 节点 2 `<->` 节点 3 `<->` 节点 4 `<->` 节点 5
    * 特点
        * 可以快速找到一个节点的下一个节点（单向链表也可以）
        * 可以快速的找到一个节点的上一个节点
        * 可以快速去掉链表中的某一个节点
* 实现一个双向链表
    * 实现链表节点
        * 可以存放 Key-Value 数据
        * 上一个节点的引用
        * 下一个节点的引用
    * 实现双向链表
        * 向头部增加节点
            * 将新节点下一个节点指向原来的头部
            * 将原来头部的上一个节点指向新节点
            * 将新节点设为新头部
            * 将新头部的上一个节点设为空
        * 向尾部增加节点
            * 将新节点上一个节点指向原来的尾部
            * 将原来尾部的下一个节点指向新节点
            * 将新节点设为新尾部
            * 将新尾部的下一个节点设为空
        * 删除任意节点
            * 删除节点
            * 改变上一节点的引用
            * 改变下一节点的引用
        * 增加任意节点
            * 即向头部或者尾部增加，前面已实现
        * 弹出头部节点
            * 将头部节点删除
            * 将头部的下一个节点设为头部
            * 将新头部的上一个节点设为空
        * 弹出尾部节点
            * 将尾部节点删除
            * 将尾部的上一个节点设为尾部
            * 将新尾部的下一个节点设为空
    * 具体实现（Python）DoublyLinkedList.py
    
        ```python
            #! -*- encoding-utf-8 -*-
      
            class Node:

                def __init__(self, key, value):
                    self.key = key
                    self.value = value
                    self.prev = None
                    self.next = None
      
                def __str__(self):
                    val = '{%d: %d}' % (self.key, self.value)
                    return val
                
                def __repr__(self):
                    val = '{%d: %d}' % (self.key, self.value)
                    return val
      
      
            class DoublyLinkedList:

                def __init__(self, capacity=0xffff):
                    self.capacity = capacity
                    self.head = None
                    self.tail = None
                    self.size = 0
      
                # 从头部添加
                def __add_head(self, node):
                    if not self.head:
                        self.head = node
                        self.tail = node
                        self.head.prev = None
                        self.head.next = None
                    else:
                        node.next = self.head
                        self.head.prev = node
                        self.head = node
                        self.head.prev = None
                    self.size += 1
                    return node
      
                # 从尾部添加
                def __add_tail(self, node):
                    if not self.tail:
                        self.head = none
                        self.tail = none
                        self.head.prev = None
                        self.head.next = None
                    else:
                        node.prev = self.tail
                        self.tail.next = node
                        self.tail = node
                        self.tail.next = None
                    self.size += 1
                    return node
      
                # 从头部删除
                def __del_head(self, node):
                    if not self.head:
                        return
                    head = self.head
                    if head.next:
                        self.head = head.next
                        self.head.prev = None
                    else:
                        self.head = self.tail = None
                    self.size -= 1
                    return head
      
                # 从尾部删除
                def __del_tail(self, node):
                    if not self.tail:
                        return
                    tail = self.tail
                    if tail.prev:
                        self.tail = tail.prev
                        self.tail.prev = None
                    else:
                        self.head = self.tail = None
                    self.size -= 1
                    return tail
      
                # 任意节点删除
                def __remove(self, node):
                    # 如果 node = None，默认删除尾部节点
                    if not node:
                        node = self.tail
                    if node == self.tail
                        self.__del_tail(node)
                    elif node == self.head:
                        self.__del_head(node)
                    else:
                        node.prev.next = node.next
                        node.next.prev = node.prev
                        slef.size -= 1
                    return node
      
                # 弹出头部节点
                def pop(self):
                  return self.__del_head()
      
                # 弹出尾部节点
                def shift(self):
                  return self.__del_tail()
      
                # 向头部添加节点
                def unshift(self, node):
                  return self.__add_head(node)
      
                # 向尾部添加节点
                def push(self, node):
                  return self.__add_tail(node)
      
                # 删除节点
                def remove(self, node=None):
                  return self.__remove(node)
      
                # 打印
                def print(self):
                    p = self.head
                    line = ''
                    while p:
                        line += '%s' % p
                        p = p.next
                        if p:
                            line += '=>'
                    print(line)
      ```
