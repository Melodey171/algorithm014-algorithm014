# 第四周学习笔记

算法题的作业每道为一个md文件，每道题包含一种或者多种解法，每种解法包含解题思路、代码实现和复杂度分析。

## 第9课 深度优先搜索和广度优先搜索

搜索-遍历：在树（图、状态集）中寻找特定节点。

- 每个节点都要访问一次

- 每个节点仅访问一次

- 对于节点的访问顺序不限
    深度优先搜索 (deep first search, DFS)
    广度优先搜索 (breadth first search, BFS)
    
- 根据优先级决定访问顺序（适用于业务场景、如推荐算法）
     启发式搜索
### 一、深度优先搜索（DFS）

#### 1.深度优先搜索使用递归或者栈实现

对于树的深度优先搜索，首先访问根节点，然后递归遍历每个子树，一个子树遍历完了再去遍历另一个子树。

对于图的深度优先搜索，从起点开始，和树的情况相似。

#### 2. 代码实现

代码网址：https://shimo.im/docs/UdY2UUKtliYXmk8t/read

- 递归写法

```
#Python
visited = set() 

def dfs(node, visited):
    if node in visited: # terminator
    	# already visited 
    	return 

	visited.add(node) 

	# process current node here. 
	...
	for next_node in node.children(): 
		if next_node not in visited: 
			dfs(next_node, visited)
```

```
//C/C++
//递归写法：
map<int, int> visited;

void dfs(Node* root) {
  // terminator
  if (!root) return ;

  if (visited.count(root->val)) {
    // already visited
    return ;
  }

  visited[root->val] = 1;

  // process current node here. 
  // ...
  for (int i = 0; i < root->children.size(); ++i) {
    dfs(root->children[i]);
  }
  return ;
}
```

```
//Java
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> allResults = new ArrayList<>();
        if(root==null){
            return allResults;
        }
        travel(root,0,allResults);
        return allResults;
    }


    private void travel(TreeNode root,int level,List<List<Integer>> results){
        if(results.size()==level){
            results.add(new ArrayList<>());
        }
        results.get(level).add(root.val);
        if(root.left!=null){
            travel(root.left,level+1,results);
        }
        if(root.right!=null){
            travel(root.right,level+1,results);
        }
    }
```
- 非递归写法

```
#Python（用栈模拟递归）
def DFS(self, tree): 

	if tree.root is None: 
		return [] 

	visited, stack = [], [tree.root]

	while stack: 
		node = stack.pop() 
		visited.add(node)

		process (node) 
		nodes = generate_related_nodes(node) 
		stack.push(nodes) 

	# other processing work 
	...
```

```
//C/C++
//非递归写法：
void dfs(Node* root) {
  map<int, int> visited;
  if(!root) return ;

  stack<Node*> stackNode;
  stackNode.push(root);

  while (!stackNode.empty()) {
    Node* node = stackNode.top();
    stackNode.pop();
    if (visited.count(node->val)) continue;
    visited[node->val] = 1;


    for (int i = node->children.size() - 1; i >= 0; --i) {
        stackNode.push(node->children[i]);
    }
  }

  return ;
}
```

![image-20200902201730911](C:\Users\86188\AppData\Roaming\Typora\typora-user-images\image-20200902201730911.png)

### 二、广度优先搜索

#### 1. 广度优先搜索使用队列（数组）实现

广度优先搜索每次遍历同一层的所有结点，可以用于最短路径求解。

#### 2. 代码实现

代码地址：https://shimo.im/docs/ZBghMEZWix0Lc2jQ/read

```
# Python
def BFS(graph, start, end):
    visited = set()
	queue = [] 
	queue.append([start]) 
	while queue: 
		node = queue.pop() 
		visited.add(node)
		process(node) 
		nodes = generate_related_nodes(node) 
		queue.push(nodes)
	# other processing work 
	...
```

```
//Java
public class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;

    TreeNode(int x) {
        val = x;
    }
}

public List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> allResults = new ArrayList<>();
    if (root == null) {
        return allResults;
    }
    Queue<TreeNode> nodes = new LinkedList<>();
    nodes.add(root);
    while (!nodes.isEmpty()) {
        int size = nodes.size();
        List<Integer> results = new ArrayList<>();
        for (int i = 0; i < size; i++) {
            TreeNode node = nodes.poll();
            results.add(node.val);
            if (node.left != null) {
                nodes.add(node.left);
            }
            if (node.right != null) {
                nodes.add(node.right);
            }
        }
        allResults.add(results);
    }
    return allResults;
}
```

```
// C/C++
void bfs(Node* root) {
  map<int, int> visited;
  if(!root) return ;

  queue<Node*> queueNode;
  queueNode.push(root);

  while (!queueNode.empty()) {
    Node* node = queueNode.top();
    queueNode.pop();
    if (visited.count(node->val)) continue;
    visited[node->val] = 1;

    for (int i = 0; i < node->children.size(); ++i) {
        queueNode.push(node->children[i]);
    }
  }

  return ;
}
```

```
//JavaScript
const bfs = (root) => {
  let result = [], queue = [root]
  while (queue.length > 0) {
    let level = [], n = queue.length
    for (let i = 0; i < n; i++) {
      let node = queue.pop()
      level.push(node.val) 
      if (node.left) queue.unshift(node.left)
      if (node.right) queue.unshift(node.right)
    }
    result.push(level)
  }
  return result
};
```

![image-20200902202554675](C:\Users\86188\AppData\Roaming\Typora\typora-user-images\image-20200902202554675.png)

## 第十课 贪心算法

### 1. 定义

贪心算法是一种在每一步选择中都采取当前状态下最好或最有（即最有利）的选择，从而希望导致结果是全局最好或者最优的算法。

### 2. 贪心算法与动态规划的区别

贪心算法与动态规划的不同在于它对于每个子问题都做出了选择，不能回退。动态规划则会保存以前的运算结果，并根据以前的结果对当前的结果对当前进行选择，有回退功能。

贪心算法可以解决一些最优化问题，但并不总是能得到最优解（有的时候是鼠目寸光，只看眼前不顾长远）。

贪心： 当下做局部最优，不可以回退，因为每个子问题都必须有最优子结构。

回溯：可以回退

动态规划：最优判断+回退

### 3. 动态规划算法的应用场景

问题能够分解成子问题去解决，子问题的最优解能地推到最终问题的最优解。这种问题最优解称为最优子结构。如求图中的最小生成树、求哈夫曼编码等。然而对于工程和生活中的问题，贪心法一般不能得到我们所要求的答案。 

一旦一个问题可以通过贪心法来解决，那么贪心法一般是解决这个问题的最好办法。由于贪心法的高效性以及其所求得的答案比较接近最优结果，贪心法也可以用作辅助算法或者直接解决一些要求结果不特别精确的问题。

### 4. 牛顿迭代法链接https://www.beyond3d.com/content/articles/8/

## 第11课 二分查找

### 1. 二分查找的前提

1. 目标函数单调性（点掉递增或者单调递减）

2. 存在上下界（bounded）

3. 能够通过索引访问（index accessible）

### 2. 二分查找的模板

例如在有序数组中查找目标值。

1. 定义下界和上界，分别为数组的左右两端下标。

2. 每次取下界和上界中间位置的下标，获取中间位置下标处的元素，并与目标值比较。

3. 如果中间位置下标处的元素与目标值相等，说明已经找到该元素，返回中间位置下表即可。

4. 如果中间位置下标处的元素小于目标值，则将下界移动到中间位置下标的右边一位。如果中间位置下标处的元素大于目标值，则将上界移动到中间位置下标的左边一位。

5. 更新下界和上界后，重复第2步到第4步，直到找到目标值，或者直到下界大于上界，则说明目标值不存在。

```
left, right = 0, len(array) - 1
while left <= right:
	mid = (left + right) // 2
	if array[mid] == target:
		break or return result
	elif array[mid] < target:
		left = mid + 1
	else:
		right = mid - 1
```



### 3. 作业：使用二分查找，寻找半有序数组中间无序的地方

和leetcode第153题思路相同。初始化下界位置和上界位置为数组的开始位置和结束位置。最小值只能在下界位置到上界位置范围内。

每次首先判断下界位置元素是否小于上界位置元素，如果小于，那么下界位置的元素就是最小值，否则进入二分查找。

根据下界位置和上界位置计算得到中间位置，考虑以下两种情况：

1. 如果中间位置元素小于下界位置元素，则最小值一定在下界位置到中间位置范围内，包含中间位置，因此将上界位置设为中间位置。

2. 如果中间位置元素大于或等于下界位置元素，则最小值一定在中间位置右侧到上街位置范围内，不包含中间位置，因此将下界位置设为中间位置右侧一位。

和leetcode第153题不同之处是，寻找无序的地方需要返回最小值得下标而不是最小值。

```
class Solution:
	def findMinIndex(self, nums):
		l, r = 0, len(nums)
		while l <= r:
			if nums[l] < nums[r]:
				return l
			else:
				mid = (l + r) // 2
				if nums[mid] == target:
					return mid
				elif nums[mid] < target:
					l = mid + 1
				else:
					r = mid - 1
		 return l			
```

## 一周总结

第四周的内容包括深度优先搜索、广度优先搜索、贪心算法和二分查找。

1. 深度优先搜索和广度优先搜索是图中常用的搜索算法，在树和二叉树中，这两种算法被广泛使用。

2. 深度优先搜索使用栈数据结构，常用递归实现，也可以手动维护栈使用迭代实现。

3. 广度优先搜索使用队列数据结构，常用迭代实现。

4. 深度优先搜索可访问到所有可达到的节点，但不保证最短距离，广度优先搜索不仅可访问到所有课到达的节点，还能保证最短距离。

### 二. 贪心算法

贪心算法的核心是“贪心”，有点“只看眼前，不顾长远，鼠目寸光”的感觉，即每一步选择都只考虑当前状态下的最有利选择。由于贪心算法不能回退，因此虽然贪心算法可以解决一些最优化问题，但是并不总是能得到最优解。

### 三. 二分查找

二分查找的适用条件包括：目标函数单调性、存在上下界、能够通过索引访问。

二分查找的常规流程：根据上下界找到中间位置，判断中间位置的值与目标值的关系，如果相等则直接返回，否则判断中间位置的哪一侧的查找范围应该保留，将查找范围缩小一半后继续查找，直到找到目标值或者下界大于上界。

实际使用二分查找时，需要对于具体问题考虑上下界的定义，二分查找的条件和更新上界和下界的规则。