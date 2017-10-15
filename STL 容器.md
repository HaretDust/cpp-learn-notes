STL容器
===

1. 序列式容器（`Sequence containers`），每个元素都有固定位置 -- 取决于插入时机和地点，和元素值无关，`vector`、`deque`、`list`

    >Vectors：将元素置于一个动态数组中加以管理，可以随机存取元素（用索引直接存取），数组尾部添加或移除元素非常快速。但是在中部或头部安插元素比较费时；

    >Deques：是“double-ended queue”的缩写，可以随机存取元素（用索引直接存取），数组头部和尾部添加或移除元素都非常快速。但是在中部或头部安插元素比较费时；      

    >Lists：双向链表，不提供随机存取（按顺序走到需存取的元素，O(n)），在任何位置上执行插入或删除动作都非常迅速，内部只需调整一下指针；

2. 关联式容器（`Associated containers`），元素位置取决于特定的排序准则，和插入顺序无关，`set`、`multiset`、`map`、`multimap`

    > Sets/Multisets：内部的元素依据其值自动排序，Set内的相同数值的元素只能出现一次，Multisets内可包含多个数值相同的元素，内部由二叉树实现（实际上基于红黑树(RB-tree）实现），便于查找；

    >Maps/Multimaps：Map的元素是成对的键值/实值，内部的元素依据其值自动排序，Map内的相同数值的元素只能出现一次，Multimaps内可包含多个数值相同的元素，内部由二叉树实现（实际上基于红黑树(RB-tree）实现），便于查找；

另外有其他容器 `hash_map` ,`hash_set`,`hash_multiset`,`hash_multimap`

  容器的比较：

\ 	| Vector | Deque | List | Set | MultiSet | Map |	MultiMap
--- | --- | --- | --- | --- | --- | --- | ---
内部结构 | dynamic array	| array of arrays |	double linked list	| binary tree	| binary tree	| binary tree | binary tree
随机存取 | Yes	| Yes	| No	| No	| No |	Yes(key)	| No
搜索速度	| 慢	| 慢	| 很慢	| 快	| 快	| 快	|快
快速插入移除	| 尾部	| 首尾	| 任何位置	| --	| --	| --	| --
