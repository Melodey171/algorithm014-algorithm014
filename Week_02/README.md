# 第二周学习笔记

算法题的作业每道题为一个md文件，每道题包含一种或多种解法，每种解法都包含解题思路、代码实现和复杂度分析。

关于HashMap的小总结在第5课的最后一部分。

## 第五课 哈希表、映射、集合

### 一. 哈希表（hash table）

#### 1. 定义

哈希表（Hash table），也叫散列表，是根据关键码值（Key value）而直接进行访问的数据结构。

它通过把关键码值映射到表中一个位置来访问记录，以加快查找的速度。这个映射函数叫作散列函

数（Hash Function），存放记录的数组交哈希表（或散列表）。

#### 2. 工程实践

电话号码本、用户信息表、缓存（LRU Cache）、键值对存储（Redis）

#### 3. 哈希碰撞（Hash Collisions）

如果不同的键经过哈希函数的计算得到的值相同，就产生哈希碰撞，解决哈希碰撞可以使用开放地址法、再哈希法、链地址法（拉链发）、建立一个公共溢出区的方法，其中拉链法（串联）是一个很好的解决方法，但是如果链表过长则会影响时间复杂度，但设计很好的情况下，平均的时间复杂度为O(1)。

#### 4. 复杂度分析

哈希表的Average Access、Search、Insertion、Deletion是O(1)，Worst状态是O(n)。

### 二. 映射

Map：将键映射到值的对象（key-value对）。一个映射不能包含重复的键；每个键最多只能映射到一个值。

常用的具体类有HashMap和TreeMap。

- HashMap的key无序，各项操作的时间复杂度为O(1)。
- TreeMap的key有序，各项操作的时间复杂度为O(log n)。

Map 、Set ：interface

### 三. 集合

Set：一个无序不重复元素集。

常用的具体类有HashSet和TreeSet。

- HashSet的元素无序，各项操作的时间复杂度为O(1)。
- TreeSet的元素有序，各项操作的时间复杂度为O(log n)。

### 四. 作业(HashMap小结)

1.HashMap是线程不安全的（HashMap是异步的，HashTable是同步的）。

2.HashMap采用了数组和链表的数据结构，能在查找的时继承数组的线性查找，在修改的时候继承链表的寻址修改。

3.HashMap是一个散列桶（数组和链表），以键值对的方式进行存储的。

4.Hashmap是通过get()方法来获取对象的。该方法根据key生成一个hash值，判断是否有对应的value，有则返回value，没有则返回null。该方法调用方法getNode，具体操作如下。

-根据(n - 1) & hash判断key是否存在，如果key不存在则返回null。

- 如果key存在，首先检查第一个节点，如果第一个节点满足要求，直接返回第一个节点。
- 如果第一个节点不满足要求，则判断是红黑树还是链表，并根据对应的结构查找满足要求的节点并返回。

5.Hashmap是通过put()方法存储对象的，当我们存储时我们把键值对传给put()方法时，他调用键的HashCode()方法来计算hashCode值，然后再找到对应的bucket位置存储对象。当获取对象时，通过调用键的Equals()方法找到正确的键值对，来返回值对象，具体操作如下。

- 将table赋给tab，并判断tab是否为空或者长度为零，如果是则调用resize方法并更新tab及其长度n。
- 根据(n - 1) & hash计算i，获得tab的第i个元素。如果为空，则直接插入。
- 如果不为空，则可能有两种情况。第一种情况是hash值重复，即已经有key对应的value。第二种情况是发生哈希碰撞，则以链表形式存入，或者在链表过长的情况下转换成红黑树。
- 如果key已经存在，则用value更新键值对，并添加到table中。

6.Hashmap通过链表的方式解决哈希碰撞，当发生碰撞时，对象会存储在链表的下一个节点中。在JDK8中，如果链表长度达到8，会转化成红黑树以提升效率。

## 第六课 树、二叉树、二叉搜索树

### 一. 树

树是二维数据结构。树由节点和边组成，每条边连接两个节点，两个节点之间的关系为父节点和子节点。

树包含一个根节点，位于第0层，其余的节点依次位于第1层、第2层、第3层等。

链表是特殊化的树，树是特殊化的图。

常见的二维数据结构：树、图。

树出现的本质是人类生活在二维的空间？不是三维吗？

Linked List（有头、尾、指针）是特殊化的Tree，Tree是特殊化的Graph，树是没环的。

拓展：阿尔法狗（棋盘）

### 二. 二叉树

#### 1. 定义

二叉树是为了加速提出来的，通常加速的方法是升维。

二叉树：每个节点最多有两个子节点，分别为左子节点和右子节点。

二叉树的遍历有三种：前序遍历、中序遍历、后序遍历。

#### 2. 树节点的定义

```
Python
class TreeNode: 
	def __init__(self, val): 
		self.val = val 
		self.left, self.right = None, None
		
C++
struct TreeNode { 
	int val; 
	TreeNode *left; 
	TreeNode *right; 
	TreeNode(int x) : val(x), left(NULL), right(NULL) {} 
}

 
Java
public class TreeNode {
	public int val;
	public TreeNode left, right;
	public TreeNode(int val) {
		this.val = val;
		this.left = null;
		this.right = null; 
	} 
}
```

#### 3. 二叉树遍历 Pre-order/In-order/Post-order

1.前序（Pre-order）：根-左-右
2.中序（In-order）：左-根-右
3.后序（Post-order）：左-右-根

#### 4. Pre-order/In-order/Post-order示例代码
```
def preorder(self, root): 
	if root: 
		self.traverse_path.append(root.val) 
		self.preorder(root.left) 
		self.preorder(root.right)
		
def inorder(self, root):
	if root: 
		self.inorder(root.left) 
		self.traverse_path.append(root.val) 
		self.inorder(root.right)
		
def postorder(self, root):
	if root: 
		self.postorder(root.left) 
		self.postorder(root.right) 
		self.traverse_path.append(root.val)
```
### 三. 二叉搜索树 Binary Search Tree
#### 1. 定义

二叉搜索树，也称二叉排序树、有序二叉树（Ordered Binary Tree）、排
序二叉树（Sorted Binary Tree），是指一棵空树或者具有下列性质的二叉
树：

1. 左子树上所有结点的值均小于它的根结点的值；
2. 右子树上所有结点的值均大于它的根结点的值；
3. 以此类推：左、右子树也分别为二叉查找树。 （这就是 重复性！）
中序遍历：升序排列

#### 2. 二叉搜索树常见操作
1. 查询
2. 插入新结点（创建）
3. 删除
Demo: https://visualgo.net/zh/bst

#### 3. 复杂度分析
Average操作都是O(logn)，Worst情况下是O(n)。

#### 4. 思考题

树的面试题解法一般都是递归，为什么？

### 四. 堆 (Heap)和二叉堆（Binary Heap）

#### 1. 定义

Heap：是可以迅速找到一堆数中的最大或者最小值得数据结构。

二叉堆是一种特殊的堆，二叉堆是完全二元树（二叉树）或者是近似完全二元树（二叉树）。

#### 2.分类

常见的堆有二叉堆、斐波那契堆等。

二叉堆有两种：最大堆和最小堆。将父结点的键值总是大于或等于任何一个子节点的键值（根节点最大）的堆叫大顶堆或者大根堆；父结点的键值总是小于或等于任何一个子节点的键值（根节点最小）的叫做小顶堆或小根堆。

假设是大顶堆，常见操作(API)：

find-max: O(1)
delete-max:O(logn)
insert(create):O(logn) or O(1)

不同的实现比较：https://en.wikipedia.org/wiki/Heap_(data_structure)

#### 3. 实现

通过完全二叉树来实现（注意：不是二叉搜索树）。

1. 完全二叉树是一种特殊的二叉树，满足以下要求：

- 所有叶子节点都出现在 k 或者 k-1 层，而且从 1 到 k-1 层必须达到最大节点数；

- 第 k 层可以不是满的，但是第 k 层的所有节点必须集中在最左边。 需要注意的是不要把完全二叉树和“满二叉树”搞混了，完全二叉树不要求所有树都有左右子树，但它要求：

- 任何一个节点不能只有左子树没有右子树

- 叶子节点出现在最后一层或者倒数第二层，不能再往上

2. 二叉堆（大顶）它满足下列性质：
- 是一颗完全树
- 树中任意节点的值总是大于或者等于其子节点的值。

注：
-  实现比较容易，但在堆里算是比较low的；
- 二叉搜索树也可以实现堆，但是find-min的时间复杂度变成O(logn)了。

### 五、图

图：由点和边组成的数据结构。

点的属性包含度，分为入度和出度，以及点与点之间存在是否连通的关系。

边的属性包含有向和无向，以及权重。

图的表示法有两种，邻接矩阵和邻接表。

图的常用算法：广度优先搜索、深度优先搜索。

图的高级算法：连通图个数、拓扑排序、最短路径、最小生成树。



