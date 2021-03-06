# chapter  9 迭代器与组合模式

> 目的: 在迭代的时候不需要考虑内部的具体实现, 这样可以统一迭代接口
>
> 举例: 比如数组的遍历一般是根据 下标来进行遍历的, 链表的遍历是根据 next 是否为 NULL 来决定的. 使用迭代器, 就可以将遍历的接口统一为 iterator.



* 如何实现迭代器模式
  * 现状: 多个不同的类, 都需要迭代
  * 做法: 
    * 类里都有个 `createIterator` 方法, 为每个类实现一个 `迭代器类` , 所有的迭代器类继承同一个接口. 这样迭代器接口就统一了. 
    * 如果每个可以迭代的类都继承自 同一个 `Iterable` 接口, 那么所有可迭代的类也统一了.





* 抽象一下该原则更像是 **(谓语/行为)** 的统一, 如果某一行为 作用的类多重多样, 且不同的类对该行为的 具体执行方式也不一样. 那不同的类就应该实现相同的行为接口.
* 感觉 设计模式的设计应该自顶向下, 从高层到低层一层层抽象



**组合模式**

* 菜单里面有菜品, 菜单里面同样有子菜单
  * 菜品 和 菜单 同样实现一个 `Component` 接口
    * 实现接口的类可以加个标记来表示当前是 菜品 还是 菜单.
    * 添加 和 迭代的时候变的很容易
* 获取可以使用类似 `pytorch` `Module` 的设计方式, 
  * `Module` 里面有 `parameter`, `Module` 里面也有 `Module`, `pytorch` 采用两个 `list` 来存储这两个东西.



## 设计原则

* 一个**类**应该只有一个引起变化的原因. 
  * **(随着系统的成长, 时刻观察有没有迹象显式某个类改变的原因超过一个)**

## 设计模式

* **迭代器模式** : 提供一种方法用于顺序的访问聚合对象中的各个元素, 而又不暴露其内部的表示. `通过返回一个迭代器对象来实现`
* **组合模式**: 要解决的问题: 有一个 list, list 里面的元素也可能是 list, 如此递归反复
  * 组合模式允许我们将对象组合成树形结构来表现 整体部分 的层次结构. 组合能让客户以一致的方式处理个别对象以及对象组合.
  * 组合模式的基本单元可以设计成一个 **树 (多叉树)**的节点(`Node`) 