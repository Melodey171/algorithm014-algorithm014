# 第一周学习笔记

算法题的作业每道题为一个md文件，每道题包含一种或多种解法，每种解法都包含解题思路、代码实现和复杂度分析。

改写Deque代码和分析源码的作业在README的最后。
## 预习课+开课直播

###  日常做题步骤

- 5-10分钟，读题思考
- 有思路：马上做和写代码，没思路，马上看题解（国内站、国际站）
- 默写背诵、熟练，然后自己开始写（闭卷）
- 重复，过遍数（指定打卡表）

刻意练习，**记忆**+理解

###  面试做题的步骤

- 和面试官确认好题目的意思
- 说出所有的解题方法，并说出不同解法的算法流程
- 3.分析time complexity and space complexity
- 4.选择一种方法，按照算法流程开始写代码，先写出大框架，然后去debug

## 第3课  数组、链表、跳表

### 一、数组

申请数组时，在内存中开辟连续地址。

由于地址连续，可在O(1)时间内查询任何下标的元素。

但是为了维持地址连续的特点，插入和删除元素的时间复杂度较高。

1. 如果在数组末尾插入元素和删除元素，时间复杂度都是O(1)。

2. 如果在数组的其余位置插入元素，则需要首先将从指定位置到末尾的所有元素后移一位，然后在指定位置插入元素，时间复杂度是O(n)，其中n是数组长度。

3. 如果在数组的其余位置删除元素，则在删除元素后，需要将从指定位置后一位到末尾的所有元素前移一位，时间复杂度是O(n)，其中n是数组长度。

具体实现类：ArrayList。

### 二、链表

链表的内存地址不连续，链表中的每个元素是一个节点，每个节点都有相应的值。链表有两种类型。

1. 节点包含指向下一个节点的指针，以这样的节点组成的链表为单向链表。

2. 节点包含指向上一个节点的指针和指向下一个节点的指针，以这样的节点组成的链表为双向链表。

链表中，通常需要指定头节点和尾节点。

在链表中插入和删除元素时，只需要改变相邻节点的指针指向的节点，因此时间复杂度都是O(1)，无论插入和删除的元素是在链表的头部、尾部还是中间。

但是由于链表的内存地址不连续，因此不可像数组那样根据下标直接查询元素，而是需要遍历节点才能查询元素，查询元素的时间复杂度是O(n)，其中n是链表中的元素数量。

具体实现类：LinkedList，为双向链表。

### 三、数组和链表的性能比较

- 数组的查询元素的时间复杂度是O(1)，插入和删除元素的时间复杂度是O(n)。

- 链表的查询元素的时间复杂度是O(n)，插入和删除元素的时间复杂度是O(1)。

基于数组和链表的性能特点，可以得出以下分析。

- 当需要经常查询元素时，使用数组更合适。

- 当需要经常插入和删除元素时，使用链表更合适。

### 四、跳表

跳表的实现基于链表，其目的是降低链表的查询元素的时间复杂度。只适用于元素有序的情况。

跳表对标平衡二叉搜索树和二分查找，插入、删除、查询元素的时间复杂度都是O(log n)，其中n是元素数量。

跳表通过升维的方式进行加速，从一维到二维，包含更多信息。具体实现为在原始链表的基础上增加多级索引，每级索引包含的节点数量依次减少。

跳表的空间复杂度和数组、链表一样是O(n)，但是由于跳表有多级索引，实际使用空间超过数组和链表。

## 第4课  栈、队列、双端队列、优先队列

### 一、栈

Stack：后进先出，添加、删除元素的时间复杂度是O(1)。

由于元素无序，查询的时间复杂度是O(n)，其中n是元素数量。

### 二、队列

Queue：先进先出，添加、删除元素的时间复杂度是O(1)。

由于元素无序，查询的时间复杂度是O(n)，其中n是元素数量。

### 三、双端队列

Deque即Double-Ended Queue：结合了栈和队列的特点，可在头和尾两端添加和删除元素。

双端队列的各项操作的时间复杂度和栈与队列相同，添加、删除元素的时间复杂度是O(1)，查询的时间复杂度是O(n)，其中n是元素数量。

### 四、优先队列

Priority Queue：元素具有优先级顺序，每次取出优先级最高的元素（例如最大值或最小值）。

添加元素的时间复杂度是O(1)，取出元素的时间复杂度是O(log n)，其中n是元素数量。

底层的具体实现的数据结构较为复杂和多样：可用heap、bst、theap去实现。

### 五、API分析

API全称又叫做Application Programming Interface，简称应用程序接口。

接口调用就是用python的requests库来访问，基本就是get或者post请求，有的接口会加密，然后我们就得用对方提供给我们的公钥加密或解密，然后在配上相应的参数进行访问。

我们要的数据就在请求后的返回结果中，我见过的基本上都是json格式解析的，所以请求后可以用requests自带的json函数来解析它，然后将要的数据提取出来即可，访问一次得到一条数据。

### 六、作业

#### 作业1  改写Deque

改写后的代码：
```python
class Deque():
    def __repr__(self):
        return str(self.list)
    
    def __init__(self):
        self.list = []
        
    def addfirst(self, num):
        self.list = [num] + self.list
    
    def addlast(self, num):
        self.list.append(num)
        
    def popleft(self):
        if not self.list: raise IndexError("pop from empty list")
        x = self.list[0]
        self.list.__delitem__(0)
        return x
    
    def popright(self):
        if not self.list: raise IndexError("pop from empty list")
        x = self.list[-1]
        self.list.__delitem__(-1)
        return x
```


#### 作业2  分析Queue和Priority Queue的源码（参照了同学的想法）

```python
class Queue:
    '''创建一个给定maxsize的队列.
    如果 maxsize <= 0, 队列大小是无穷大（下面可以看到默认值为0）.
    '''
    
    def __init__(self, maxsize=0):
        self.maxsize = maxsize
        self._init(maxsize)
		
        # mutex——互斥锁，属于线程锁。
        # 任何时候队列在发生改变时，必须加持有互斥锁。
        # 所有需要加锁的方法，在返回值之前必须释放锁。
        # 互斥锁在以上三种状态（|<--1-->|加锁|<--2-->|释放锁|<--3-->|）时是共享的。
        # 因此，获取或者释放状态时，也获取和释放了互斥锁
        self.mutex = threading.Lock()
		
        # 
        # Notify not_empty whenever an item is added to the queue; a
        # thread waiting to get is notified then.
        self.not_empty = threading.Condition(self.mutex)

        # Notify not_full whenever an item is removed from the queue;
        # a thread waiting to put is notified then.
        self.not_full = threading.Condition(self.mutex)

        # Notify all_tasks_done whenever the number of unfinished tasks
        # drops to zero; thread waiting to join() is notified to resume
        self.all_tasks_done = threading.Condition(self.mutex)
        self.unfinished_tasks = 0

    def task_done(self):
        '''Indicate that a formerly enqueued task is complete.
		
		
        Used by Queue consumer threads.  For each get() used to fetch a task,
        a subsequent call to task_done() tells the queue that the processing
        on the task is complete.

        If a join() is currently blocking, it will resume when all items
        have been processed (meaning that a task_done() call was received
        for every item that had been put() into the queue).

        Raises a ValueError if called more times than there were items
        placed in the queue.
        '''
        with self.all_tasks_done:
            unfinished = self.unfinished_tasks - 1
            if unfinished <= 0:
                if unfinished < 0:
                    raise ValueError('task_done() called too many times')
                self.all_tasks_done.notify_all()
            self.unfinished_tasks = unfinished

    def join(self):
        '''阻塞，直到Queue中的所有对象都被获取并处理了。
        Blocks until all items in the Queue have been gotten and processed.
		
		任何时候一个对象被加进Queue中后，未完成任务的计数都会增加。
		任何时候一个消费者线程调用task_done()方法表明一个对象被获取且工作完毕了，这个计数都会减少。
        The count of unfinished tasks goes up whenever an item is added to the
        queue. The count goes down whenever a consumer thread calls task_done()
        to indicate the item was retrieved and all work on it is complete.
		
		当未完成任务的计数减少到0时，join()方法的阻塞被释放。
        When the count of unfinished tasks drops to zero, join() unblocks.
        '''
        with self.all_tasks_done:
            while self.unfinished_tasks:
                self.all_tasks_done.wait()

    def qsize(self):
        '''返回队列的大概长度 (不可靠!).'''
        with self.mutex:
            return self._qsize()

    def empty(self):
        '''判断一个队列是否为空，是返回True，否返回False (不可靠!).
		
		这个方法可能会在未来被删除。直接使用qsize()方法来判断队列是否为空。
        This method is likely to be removed at some point.  Use qsize() == 0
        as a direct substitute, but be aware that either approach risks a race
        condition where a queue can grow before the result of empty() or
        qsize() can be used.

        To create code that needs to wait for all queued tasks to be
        completed, the preferred technique is to use the join() method.
        '''
        with self.mutex:
            return not self._qsize()

    def full(self):
        '''Return True if the queue is full, False otherwise (not reliable!).

        This method is likely to be removed at some point.  Use qsize() >= n
        as a direct substitute, but be aware that either approach risks a race
        condition where a queue can shrink before the result of full() or
        qsize() can be used.
        '''
        with self.mutex:
            return 0 < self.maxsize <= self._qsize()

    def put(self, item, block=True, timeout=None):
        '''Put an item into the queue.
		把一个对象放进队列。
		
		如果可选参数 block 为True, 并且 timeout 为 None，那么操作会阻塞，直到队列有可用空间。
		如果 timeout 为非负整数，则会阻塞指定时间，如果这段时间都没有位置释放，则会报错。
		
		其他情况（block为false），队列如果有空槽位则直接放进去，没有则直接报错。
		
        If optional args 'block' is true and 'timeout' is None (the default),
        block if necessary until a free slot is available. If 'timeout' is
        a non-negative number, it blocks at most 'timeout' seconds and raises
        the Full exception if no free slot was available within that time.
        Otherwise ('block' is false), put an item on the queue if a free slot
        is immediately available, else raise the Full exception ('timeout'
        is ignored in that case).
        '''
        with self.not_full:
            if self.maxsize > 0:
                if not block:
                    if self._qsize() >= self.maxsize:
                        raise Full
                elif timeout is None:
                    while self._qsize() >= self.maxsize:
                        self.not_full.wait()
                elif timeout < 0:
                    raise ValueError("'timeout' must be a non-negative number")
                else:
                    endtime = time() + timeout
                    while self._qsize() >= self.maxsize:
                        remaining = endtime - time()
                        if remaining <= 0.0:
                            raise Full
                        self.not_full.wait(remaining)
            self._put(item)
            self.unfinished_tasks += 1
            self.not_empty.notify()

    def get(self, block=True, timeout=None):
        '''Remove and return an item from the queue.

        If optional args 'block' is true and 'timeout' is None (the default),
        block if necessary until an item is available. If 'timeout' is
        a non-negative number, it blocks at most 'timeout' seconds and raises
        the Empty exception if no item was available within that time.
        Otherwise ('block' is false), return an item if one is immediately
        available, else raise the Empty exception ('timeout' is ignored
        in that case).
        '''
        with self.not_empty:
            if not block:
                if not self._qsize():
                    raise Empty
            elif timeout is None:
                while not self._qsize():
                    self.not_empty.wait()
            elif timeout < 0:
                raise ValueError("'timeout' must be a non-negative number")
            else:
                endtime = time() + timeout
                while not self._qsize():
                    remaining = endtime - time()
                    if remaining <= 0.0:
                        raise Empty
                    self.not_empty.wait(remaining)
            item = self._get()
            self.not_full.notify()
            return item

    def put_nowait(self, item):
        '''Put an item into the queue without blocking.

        Only enqueue the item if a free slot is immediately available.
        Otherwise raise the Full exception.
        '''
        return self.put(item, block=False)

    def get_nowait(self):
        '''Remove and return an item from the queue without blocking.

        Only get an item if one is immediately available. Otherwise
        raise the Empty exception.
        '''
        return self.get(block=False)

    # Override these methods to implement other queue organizations
    # (e.g. stack or priority queue).
    # These will only be called with appropriate locks held

    # 实例化类时调用的函数，可以看到，Queue实际上是一个deque()对象
    # Initialize the queue representation
    def _init(self, maxsize):
        self.queue = deque()

    # 返回队列的长度
    def _qsize(self):
        return len(self.queue)

    # 在队列中添加(append)一个新对象
    def _put(self, item):
        self.queue.append(item)

    # 从队列中获取(popleft)一个对象
    def _get(self):
        return self.queue.popleft()
```

可以看到核心的 put() 和 get() 两个方法，其实用的就是 append 和 popleft 来实现的，为什么有 popleft 方法？这其实是因为 **Queue 其实是一个 deque 对象** （在_init()方法中定义了）。

##### **deque**

继续深挖，找不到 deque 的 .py 源码，于是发现 python 的 collections 库其实是用c语言（cpython）实现的，在这里：[_collectionsmodule.c](https://github.com/python/cpython/blob/master/Modules/_collectionsmodule.c) 。源码太长，看不懂太c，就不继续深挖了。由于deque是c语言的实现，所以我们有理由相信deque具有非常不错的性能，以后可以多多使用。

##### **PriorityQueue**

位于`Miniconda3\Lib\queue.py`文件 `Line 220-236`

```python
class PriorityQueue(Queue):
    '''Variant of Queue that retrieves open entries in priority order (lowest first).

    Entries are typically tuples of the form:  (priority number, data).
    '''

    def _init(self, maxsize):
        self.queue = []

    def _qsize(self):
        return len(self.queue)

    def _put(self, item):
        heappush(self.queue, item)

    def _get(self):
        return heappop(self.queue)
```

可以看到PriorityQueue本身其实就是一个Queue的类继承，它改写了Queue的 \_put() 和 \_get() 方法，使用了 heappush 和 heappop 方法替代了原来的 append 和 pop，在插入和获取对象的同时，会对 Queue 实例中的元素进行排序。查阅 heapq 库，可以看到这两个方法：

```python
def heappush(heap, item):
    """Push item onto heap, maintaining the heap invariant."""
    heap.append(item)
    _siftdown(heap, 0, len(heap)-1)

def heappop(heap):
    """Pop the smallest item off the heap, maintaining the heap invariant."""
    lastelt = heap.pop()    # raises appropriate IndexError if heap is empty
    if heap:
        returnitem = heap[0]
        heap[0] = lastelt
        _siftup(heap, 0)
        return returnitem
    return lastelt
```

这两个方法基本上就是堆的特有方法，可以看到 python 的 **heap 其实就是 python 基本的 list 对象**，使用pop，append等方法来操作。

##### heapq

进一步看以下 heapq（[文档](https://docs.python.org/3.8/library/heapq.html)） 这个库，这个库提供了有如下方法：

```python
__all__ = ['heappush', 'heappop', 'heapify', 'heapreplace', 'merge',
           'nlargest', 'nsmallest', 'heappushpop']
```

最常用的应该是 `heappush` 和 `heappop` 这两个。还有`heapify`（将传入对象转化为满足堆的顺序）

例如：`heapify`

```python
>>> l = [6,3,5,2,1,8,7]
>>> heapq.heapify(l)
>>> l
[1, 2, 5, 6, 3, 8, 7]
```

`heappush`提供插入一个元素到堆中的功能：

```python
>>> heapq.heappush(l, 4)
>>> l
[1, 2, 5, 4, 3, 8, 7, 6]
```

`heappop`提供从堆中获取一个元素的功能，且获取的元素为堆中最小值

```python
>>> heapq.heappop(l)
1
>>> heapq.heappop(l)
2
>>> heapq.heappop(l)
3
>>> heapq.heappop(l)
4
>>> heapq.heappop(l)
5
>>> heapq.heappop(l)
6
>>> heapq.heappop(l)
7
>>> heapq.heappop(l)
8
```

`nlargest`提供查询堆中最大n个元素的功能

```python
>>> heapq.nlargest(4, l)
[8, 7, 6, 5]
# 下面两个是乱打的列表，不满足堆结构，但依旧能查询，推测会先自动heapify
>>> heapq.nlargest(4, [1,3,6,3,8,2,2,3,6,9])
[9, 8, 6, 6]
>>> heapq.nlargest(4, [19,3,6,3,8,2,2,3,6,9])
[19, 9, 8, 6]
# 支持key功能
>>> heapq.nlargest(4, [(1,"D"),(2,"C")], key=lambda x: x[1])
[(1, 'D'), (2, 'C')]
```

`heapreplace`，pop一个，然后push一个。

官方还提供了一个用heapq库进行堆排序的例子：

```python
>>> def heapsort(iterable):
...     h = []
...     for value in iterable:
...         heappush(h, value)
...     return [heappop(h) for i in range(len(h))]
...
>>> heapsort([1, 3, 5, 7, 9, 2, 4, 6, 8, 0])
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

另外，heapq这个库仅仅实现了最小堆，而没有最大堆这个功能，如果要用heapq做最大堆，有一个trick，就是用负数来存元素：

```python
# Python3 program to demonstrate working of heapq 

from heapq import heappop, heappush, heapify 
  
# Creating empty heap 
heap = [] 
heapify(heap) 
  
# Adding items to the heap using heappush 
# function by multiplying them with -1 
heappush(heap, -1 * 10) 
heappush(heap, -1 * 30) 
heappush(heap, -1 * 20) 
heappush(heap, -1 * 400) 
  
# printing the value of maximum element 
print("Head value of heap : "+str(-1 * heap[0])) 
  
# printing the elements of the heap 
print("The heap elements : ") 
for i in heap: 
    print(-1 * i, end = ' ') 
print("\n") 
  
element = heappop(heap) 
  
# printing the elements of the heap 
print("The heap elements : ") 
for i in heap: 
    print(-1 * i, end = ' ') 
```

