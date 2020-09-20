# 第六周学习笔记

算法题的作业每道题为一个md文件，每道题包含一种或多种解法，每种解法都包含解题思路、代码实现和复杂度分析；笔记后面部分展示了6道股票系列的python和Java解法。

## 第12课 动态规划

### 一、动态规划

动态规划实质解决的是递归或分治问题。

和普通的递归或分治问题的不同之处在于，动态规划具有最优子结构。

动态规划的核心是将一个复杂的问题分解成简单的子问题（用递归的方式）。动态规划和分治是密切相关的。

### 二、动态规划和递归/分治的关键点

动态规划和递归/分治没有根本上的区别，关键看有无最优子结构。

共性：找到重复子问题。

差异性：最优子结构、中途可以淘汰次优解。

## 一周总结

### 1. 维基百科定义和实现：

#### (1). 定义：

动态规划（Dynamic Programming，DP）：既是 数学优化方法又是 计算机编程方法。

（个人理解为 本质 上就是一种 动态递推，是通过 编程 去实现 数学归纳法）

#### (2). DP实现：

Simplifying a complicated problem by breaking it down into simpler sub-problems in a recursive manner （通过 递归的方式 把一个 复杂的问题 拆分成 子问题 来简化复杂问题）

### 2. 递归、迭代、动态规划、分治的区别与联系

1. 递归 和 迭代 都是指 程序的实现方式（迭代是人，递归是神）
   （1）递归（A调用A），迭代（A重复调用B）
   （2）迭代 一般 有缓存机制，递归没有

2. 分治、回溯、动态规划 是指 算法，通过 递归 和 迭代 去实现
3. 分治，通过 自顶向下 求解子问题，并合并，一般通过 递归 实现
4.  动态规划，一般有 自顶向下 和 自底向上 两种做法 求解子问题，自顶向下用 记忆化递归 实现，自底向上 一般通过 迭代 实现（最为推荐的做法），两种做法都不会重复计算子问题
5.  适合dp求解 最优化的问题具备的两个要素：最优子结构 和 重叠子问题
6. 分治法 —— 各子问题独立；动态规划 —— 各子问题重叠

### 3. 动态规划的三大特征

1.  最优子结构：当问题的最优解 包含其子问题的最优解 时，称问题具有最优子结构
   opt[n] = best_of([opt[n-1], opt[n-2], ...)

2. 重叠子问题：dp利用了 子问题 的重叠性，对每个子问题 只计算一次并存放，在以后尽可能多的利用这些子问题的解（时间复杂度降低的主要原因）

3. 无后效性：即当前状态如果 确定，以后过程的演变将 不再受当前状态以前的各状态 和 以前的决策影

### 4.动态规划的4个做题步骤

      1. 定义子问题：即当前状态，也是 需要存储的 每个 子问题的解，如 d[i]
      2. 找到问题到子问题的递推关系，即 状态转移关系（dp方程），如   dp[i]=F(dp[i-1])， Fib: dp[i] = dp[n-1] + dp[n-2]，二维路径： dp[i][j] = dp[i+1][j] + dp[i][j+1] 
      3. 确定计算顺序，并为 状态数组进行初始化
      4. 通过状态数组得到 原问题的解，如max(dp), dp[n-1]

### 5. 动态规划思维要点

1. 打破自己的思维惯性，形成机器思维。
2. 理解复杂逻辑的关键。
3. 也是职业进阶的要点要领。

### 6. 动态规划的解题框架

动态规划题可以通过自顶向下或者自底向上的方法实现。

自顶向下的实质是递归和分治加上记忆化，对于初学者而言，可以使用自顶向下的方法，更好理解。

达到一定水平之后，就应该使用自底向上的方法，通过循环实现。



# 股票买卖系列通解专题

## 一、通用情况

这个想法基于如下问题：给定一个表示每天股票价格的数组，什么因素决定了可以获得的最大收益？

相信大多数人可以很快给出答案，例如「在哪些天进行交易以及允许多少次交易」。这些因素当然重要，在问题描述中也有这些因素。然而还有一个隐藏但是关键的因素决定了最大收益，下文将阐述这一点。

首先介绍一些符号：

- 用 `n` 表示股票价格数组的长度；

- 用`i` 表示第 i 天（i 的取值范围是 0 到 n - 1）；

- 用`k`表示允许的最大交易次数；

- 用 `T[i][k]` 表示在第`i`  天结束时，最多进行 k 次交易的情况下可以获得的最大收益。

基准情况是显而易见的：`T[-1][k] = T[i][0] = 0`，表示没有进行股票交易时没有收益（注意第一天对应 `i = 0`，因此` i = -1` 表示没有股票交易）。现在如果可以将 T[i][k] 关联到子问题，例如`T[i - 1][k]`、`T[i][k - 1]`、`T[i - 1][k - 1]` 等子问题，就能得到状态转移方程，并对这个问题求解。如何得到状态转移方程呢？

最直接的办法是看第 i 天可能的操作。有多少个选项？答案是三个：买入、卖出、休息。应该选择哪个操作？答案是：并不知道哪个操作是最好的，但是可以通过计算得到选择每个操作可以得到的最大收益。假设没有别的限制条件，则可以尝试每一种操作，并选择可以最大化收益的一种操作。但是，题目中确实有限制条件，规定不能同时进行多次交易，因此如果决定在第 i 天买入，在买入之前必须持有 0 份股票，如果决定在第 i 天卖出，在卖出之前必须恰好持有 1 份股票。持有股票的数量是上文提及到的隐藏因素，该因素影响第 i 天可以进行的操作，进而影响最大收益。

因此对 `T[i][k]` 的定义需要分成两项：

- `T[i][k][0]` 表示在第 `i` 天结束时，最多进行 k 次交易且在进行操作后持有 0 份股票的情况下可以获得的最大收益；
- `T[i][k][1]` 表示在第 `i`天结束时，最多进行 k 次交易且在进行操作后持有 1 份股票的情况下可以获得的最大收益。

使用新的状态表示之后，可以得到基准情况和状态转移方程。

基准情况：

`T[-1][k][0] = 0, T[-1][k][1] = -Infinity`
`T[i][0][0] = 0, T[i][0][1] = -Infinity`

状态转移方程：

`T[i][k][0] = max(T[i - 1][k][0], T[i - 1][k][1] + prices[i])`
`T[i][k][1] = max(T[i - 1][k][1], T[i - 1][k - 1][0] - prices[i])`

基准情况中，`T[-1][k][0] = T[i][0][0] = 0` 的含义和上文相同，`T[-1][k][1] = T[i][0][1] = -Infinity` 的含义是在没有进行股票交易时不允许持有股票。

对于状态转移方程中的 `T[i][k][0]`，第`i` 天进行的操作只能是休息或卖出，因为在第 i 天结束时持有的股票数量是 0。`T[i - 1][k][0]` 是休息操作可以得到的最大收益，`T[i - 1][k][1] + prices[i]` 是卖出操作可以得到的最大收益。注意到允许的最大交易次数是不变的，因为每次交易包含两次成对的操作，买入和卖出。只有买入操作会改变允许的最大交易次数。

对于状态转移方程中的 `T[i][k][1]`，第 i 天进行的操作只能是休息或买入，因为在第 i 天结束时持有的股票数量是 1。`T[i - 1][k][1]` 是休息操作可以得到的最大收益，`T[i - 1][k - 1][0] - prices[i]` 是买入操作可以得到的最大收益。注意到允许的最大交易次数减少了一次，因为每次买入操作会使用一次交易。

为了得到最后一天结束时的最大收益，可以遍历股票价格数组，根据状态转移方程计算 `T[i][k][0]` 和 `T[i][k][1]` 的值。最终答案是 `T[n - 1][k][0]`，因为结束时持有 0 份股票的收益一定大于持有 1 份股票的收益。



#### [121. 买卖股票的最佳时机](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)

#### [122. 买卖股票的最佳时机 II](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/)

#### [123. 买卖股票的最佳时机 III](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iii/)

#### [188. 买卖股票的最佳时机 IV](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iv/)

#### [309. 最佳买卖股票时机含冷冻期](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

#### [714. 买卖股票的最佳时机含手续费](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/)



## 二、应用于特殊情况

上述六个股票问题是根据 k 的值进行分类的，其中 k 是允许的最大交易次数。最后两个问题有附加限制，包括「冷冻期」和「手续费」。通解可以应用于每个股票问题。

### 一、121. 买卖股票的最佳时机

情况一：k = 1

情况一对应的题目是「121. 买卖股票的最佳时机」。

对于情况一，每天有两个未知变量：`T[i][1][0]` 和 `T[i][1][1]`，状态转移方程如下：

`T[i][1][0] = max(T[i - 1][1][0], T[i - 1][1][1] + prices[i])`
`T[i][1][1] = max(T[i - 1][1][1], T[i - 1][0][0] - prices[i]) = max(T[i - 1][1][1], -prices[i])`

第二个状态转移方程利用了 `T[i][0][0] = 0`。

根据上述状态转移方程，可以写出时间复杂度为 O(n) 和空间复杂度为 O(n) 的解法。

```
#Python 时间O(n) 空间O(n)
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
    	#1.终止条件
        n = len(prices)
        if n == 0: return 0
        #2.确定计算顺序，状态数组进行初始化
        dp = [[0 for i in range(2)] for j in range(n)]
        # dp = [[0] * 2] * n#里层是列，外层是行
        dp[0][0] = 0
        dp[0][1] = -prices[0]
        #3.递推工作，找到状态转移关系（dp方程）
        for i in range(1, n):
            dp[i][0] = max(dp[i - 1][0], dp[i - 1][1] + prices[i])
            dp[i][1] = max(dp[i - 1][1], -prices[i])
        #4.返回原问题的解
        return dp[n - 1][0]
```

```
#Java 时间O(n) 空间O(n)
class Solution {
    public int maxProfit(int[] prices) {
        if (prices == null || prices.length == 0) {
            return 0;
        }
        int length = prices.length;
        int[][] dp = new int[length][2];
        dp[0][0] = 0;
        dp[0][1] = -prices[0];
        for (int i = 1; i < length; i++) {
            dp[i][0] = Math.max(dp[i - 1][0], dp[i - 1][1] + prices[i]);
            dp[i][1] = Math.max(dp[i - 1][1], -prices[i]);
        }
        return dp[length - 1][0];
    }
}
```

如果上述代码如果注意到第i天的收益只与i-1天的最大收益相关，这时的时间O(n) 空间O(1)

```
#Python 时间O(n) 空间O(1)
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
    	n = len(prices)
        if n == 0: return 0
        profit0 = 0
        profit1 = -prices[0]
        for i in range(1, n):
            profit0 = max(profit0, profit1 + prices[i])
            profit1 = max(profit1, -prices[i])
        return profit0
```

```
#Java 时间O(n) 空间O(1)
class Solution {
    public int maxProfit(int[] prices) {
        if (prices == null || prices.length == 0) {
            return 0;
        }
        int profit0 = 0, profit1 = -prices[0];
        int length = prices.length;
        for (int i = 1; i < length; i++) {
            profit0 = Math.max(profit0, profit1 + prices[i]);
            profit1 = Math.max(profit1, -prices[i]);
        }
        return profit0;
    }
}
```



### 122. 买卖股票的最佳时机 II

情况二：k 为正无穷

情况二对应的题目是「122. 买卖股票的最佳时机 II」。

如果 k 为正无穷，则 k 和 k - 1 可以看成是相同的，因此有` T[i - 1][k - 1][0] = T[i - 1][k][0]` 和 `T[i - 1][k - 1][1] = T[i - 1][k][1]`。每天仍有两个未知变量：`T[i][k][0] 和 T[i][k][1]`，其中 k 为正无穷，状态转移方程如下：

`T[i][k][0] = max(T[i - 1][k][0], T[i - 1][k][1] + prices[i])`
`T[i][k][1] = max(T[i - 1][k][1], T[i - 1][k - 1][0] - prices[i]) = max(T[i - 1][k][1], T[i - 1][k][0] - prices[i])`

第二个状态转移方程利用了 `T[i - 1][k - 1][0] = T[i - 1][k][0]`。

根据上述状态转移方程，可以写出时间复杂度为 O(n)和空间复杂度为 O(n) 的解法。

```
#Python 时间O(n) 空间O(n)
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        # 时间O(n) O(n)
        n = len(prices)
        if prices == 0: return 0
        dp = [[0 for i in range(2)] for j in range(n)]
        # dp = [[0] * 2] * n
        dp[0][0] = 0
        dp[0][-1] = -prices[0]
        for i in range(1, n):
            dp[i][0] = max(dp[i - 1][0], dp[i - 1][1] + prices[i])
            dp[i][1] = max(dp[i - 1][1], dp[i -1][0] - prices[i])
        return dp[n-1][0]
```

```
#Java 时间O(n) 空间O(n)
class Solution {
    public int maxProfit(int[] prices) {
        if (prices == null || prices.length == 0) {
            return 0;
        }
        int length = prices.length;
        int[][] dp = new int[length][2];
        dp[0][0] = 0;
        dp[0][1] = -prices[0];
        for (int i = 1; i < length; i++) {
            dp[i][0] = Math.max(dp[i - 1][0], dp[i - 1][1] + prices[i]);
            dp[i][1] = Math.max(dp[i - 1][1], dp[i - 1][0] - prices[i]);
        }
        return dp[length - 1][0];
    }
}
```

如果意识到第i天的最大收益只和第i - 1天的最大收益有关，空间复杂度可以降到O(1)

```
#Python 时间O(n) 空间O(1)
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        n = len(prices)
        if n == 0:
            return 0
        profit0 = 0
        profit1 = -prices[0]
        for i in range(1, n):
            newprofit0 = max(profit0, profit1 + prices[i])
            newprofit1 = max(profit1, profit0 - prices[i])
            profit0 = newprofit0
            profit1 = newprofit1
        return profit0
```

```
#Java 时间O(n) 空间O(1)
class Solution {
    public int maxProfit(int[] prices) {
        if (prices == null || prices.length == 0) {
            return 0;
        }
        int profit0 = 0, profit1 = -prices[0];
        int length = prices.length;
        for (int i = 1; i < length; i++) {
            int newProfit0 = Math.max(profit0, profit1 + prices[i]);
            int newProfit1 = Math.max(profit1, profit0 - prices[i]);
            profit0 = newProfit0;
            profit1 = newProfit1;
        }
        return profit0;
    }
}
```


### 123. 买卖股票的最佳时机 III

情况三对应的题目是「123. 买卖股票的最佳时机 III」。

情况三和情况一相似，区别之处是，对于情况三，每天有四个未知变量：`T[i][1][0]、T[i][1][1]、T[i][2][0]、T[i][2][1]`，状态转移方程如下：

`T[i][2][0] = max(T[i - 1][2][0], T[i - 1][2][1] + prices[i])`
`T[i][2][1] = max(T[i - 1][2][1], T[i - 1][1][0] - prices[i])`
`T[i][1][0] = max(T[i - 1][1][0], T[i - 1][1][1] + prices[i])`
`T[i][1][1] = max(T[i - 1][1][1], T[i - 1][0][0] - prices[i]) = max(T[i - 1][1][1], -prices[i])`

第四个状态转移方程利用了 `T[i][0][0] = 0`。

根据上述状态转移方程，可以写出时间复杂度为 O(n) 和空间复杂度为 O(n) 的解法。

```
#Python 时间O(n) 空间O(n)
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        n = len(prices)
        if n == 0: return 0
        dp = [[[0 for i in range(2)] for j in range(3)] for k in range(n)]
        dp[0][1][0] = 0
        dp[0][1][1] = -prices[0]
        dp[0][2][0] = 0
        dp[0][2][1] = -prices[0]
        for i in range(1, n):
            dp[i][2][0] = max(dp[i - 1][2][0], dp[i - 1][2][1] + prices[i])
            dp[i][2][1] = max(dp[i - 1][2][1], dp[i - 1][1][0] - prices[i])
            dp[i][1][0] = max(dp[i - 1][1][0], dp[i - 1][1][1] + prices[i])
            dp[i][1][1] = max(dp[i - 1][1][1], dp[i - 1][0][0] - prices[i])
        return dp[n - 1][2][0]
```

```
#Java 时间O(n) 空间O(n)
class Solution {
    public int maxProfit(int[] prices) {
        if (prices == null || prices.length == 0) {
            return 0;
        }
        int length = prices.length;
        int[][][] dp = new int[length][3][2];
        dp[0][1][0] = 0;
        dp[0][1][1] = -prices[0];
        dp[0][2][0] = 0;
        dp[0][2][1] = -prices[0];
        for (int i = 1; i < length; i++) {
            dp[i][2][0] = Math.max(dp[i - 1][2][0], dp[i - 1][2][1] + prices[i]);
            dp[i][2][1] = Math.max(dp[i - 1][2][1], dp[i - 1][1][0] - prices[i]);
            dp[i][1][0] = Math.max(dp[i - 1][1][0], dp[i - 1][1][1] + prices[i]);
            dp[i][1][1] = Math.max(dp[i - 1][1][1], dp[i - 1][0][0] - prices[i]);
        }
        return dp[length - 1][2][0];
    }
}
```

如果注意到第 `i` 天的最大收益只和第 `i - 1` 天的最大收益相关，空间复杂度可以降到 O(1)

```
#python 时间O(n) 空间O(1)
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        n = len(prices)
        if n == 0: return 0
        profitOne0 = 0
        profitOne1 = -prices[0]
        profitTwo0 = 0
        profitTwo1 = -prices[0]
        for i in range(1, n):
            profitTwo0 = max(profitTwo0, profitTwo1 + prices[i])
            profitTwo1 = max(profitTwo1, profitOne0 - prices[i])
            profitOne0 = max(profitOne0, profitOne1 + prices[i])
            profitOne1 = max(profitOne1, -prices[i])
        return profitTwo0
```

```
#Java 时间O(n) 空间O(1)
class Solution {
    public int maxProfit(int[] prices) {
        if (prices == null || prices.length == 0) {
            return 0;
        }
        int profitOne0 = 0, profitOne1 = -prices[0], profitTwo0 = 0, profitTwo1 = -prices[0];
        int length = prices.length;
        for (int i = 1; i < length; i++) {
            profitTwo0 = Math.max(profitTwo0, profitTwo1 + prices[i]);
            profitTwo1 = Math.max(profitTwo1, profitOne0 - prices[i]);
            profitOne0 = Math.max(profitOne0, profitOne1 + prices[i]);
            profitOne1 = Math.max(profitOne1, -prices[i]);
        }
        return profitTwo0;
    }
}
```

### 188.买卖股票的最佳时机 IV

情况四：k 为任意值

情况四对应的题目是「188. 买卖股票的最佳时机 IV」。

情况四是最通用的情况，对于每一天需要使用不同的 k 值更新所有的最大收益，对应持有 0 份股票或 1 份股票。如果 k 超过一个临界值，最大收益就不再取决于允许的最大交易次数，而是取决于股票价格数组的长度，因此可以进行优化。那么这个临界值是什么呢？

一个有收益的交易至少需要两天（在前一天买入，在后一天卖出，前提是买入价格低于卖出价格）。如果股票价格数组的长度为 n，则有收益的交易的数量最多为 n / 2（整数除法）。因此 k 的临界值是 n / 2。如果给定的 k 不小于临界值，即 k >= n / 2，则可以将 k 扩展为正无穷，此时问题等价于情况二。

根据状态转移方程，可以写出时间复杂度为 O(nk)和空间复杂度为 O(nk) 的解法。

```
#python 时间 O(nk) 空间O(nk)
class Solution:
    def maxProfit(self, k: int, prices: List[int]) -> int:
        n = len(prices)
        if n == 0: return 0
        if k >= n // 2:
            return self.maxProfit0(prices)
        dp = [[[0 for i in range(2)] for j in range(k + 1)] for r in range(n)]
        for i in range(1, k + 1):
            dp[0][i][0] = 0
            dp[0][i][1] = -prices[0]
        for i in range(1, n):
            for j in range(1, k + 1):
                dp[i][j][0] = max(dp[i - 1][j][0], dp[i - 1][j][1] + prices[i])
                dp[i][j][1] = max(dp[i - 1][j][1], dp[i - 1][j - 1][0] - prices[i])
        return dp[n - 1][k][0]

    def maxProfit0(self, prices: List[int]) -> int:
        n = len(prices)
        if n == 0:
            return 0
        dp = [[0 for i in range(2)] for j in range(n)]
        dp[0][0] = 0
        dp[0][1] = -prices[0]
        for i in range(1, n):
            dp[i][0] = max(dp[i - 1][0], dp[i - 1][1] + prices[i])
            dp[i][1] = max(dp[i - 1][1], dp[i - 1][0] - prices[i])
        return dp[n - 1][0]
```

```
#Java 时间 O(nk) 空间O(nk)
class Solution {
    public int maxProfit(int k, int[] prices) {
        if (prices == null || prices.length == 0) {
            return 0;
        }
        int length = prices.length;
        if (k >= length / 2) {
            return maxProfit(prices);
        }
        int[][][] dp = new int[length][k + 1][2];
        for (int i = 1; i <= k; i++) {
            dp[0][i][0] = 0;
            dp[0][i][1] = -prices[0];
        }
        for (int i = 1; i < length; i++) {
            for (int j = k; j > 0; j--) {
                dp[i][j][0] = Math.max(dp[i - 1][j][0], dp[i - 1][j][1] + prices[i]);
                dp[i][j][1] = Math.max(dp[i - 1][j][1], dp[i - 1][j - 1][0] - prices[i]);
            }
        }
        return dp[length - 1][k][0];
    }

    public int maxProfit(int[] prices) {
        if (prices == null || prices.length == 0) {
            return 0;
        }
        int length = prices.length;
        int[][] dp = new int[length][2];
        dp[0][0] = 0;
        dp[0][1] = -prices[0];
        for (int i = 1; i < length; i++) {
            dp[i][0] = Math.max(dp[i - 1][0], dp[i - 1][1] + prices[i]);
            dp[i][1] = Math.max(dp[i - 1][1], dp[i - 1][0] - prices[i]);
        }
        return dp[length - 1][0];
    }
}
```

如果注意到第 `i` 天的最大收益只和第 `i - 1` 天的最大收益相关，空间复杂度可以降到 O(k)。

```
#python 时间 O(nk) 空间O(k)
class Solution:
    def maxProfit(self, k: int, prices: List[int]) -> int:

        # 时间O(nk) 空间O(k)
        n = len(prices)
        if n == 0: return 0
        if k >= n // 2:
            return self.maxProfit0(prices)
        dp = [[0 for i in range(2)] for j in range(k + 1)]
        for i in range(1, k + 1):
            dp[i][0] = 0
            dp[i][1] = -prices[0]
        for i in range(1, n):
            for j in range(1, k + 1):
                dp[j][0] = max(dp[j][0], dp[j][1] + prices[i])
                dp[j][1] = max(dp[j][1], dp[j - 1][0] - prices[i])
        return dp[k][0]

    def maxProfit0(self, prices: List[int]) -> int:
        n = len(prices)
        if n == 0: return 0
        profit0 = 0
        profit1 = -prices[0]
        for i in range(1, n):
            newProfit0 = max(profit0, profit1 + prices[i])
            newProfit1 = max(profit1, profit0 - prices[i])
            profit0 = newProfit0
            profit1 = newProfit1
        return profit0
```

```
#Java 时间 O(nk) 空间O(k)
class Solution {
    public int maxProfit(int k, int[] prices) {
        if (prices == null || prices.length == 0) {
            return 0;
        }
        int length = prices.length;
        if (k >= length / 2) {
            return maxProfit(prices);
        }
        int[][] dp = new int[k + 1][2];
        for (int i = 1; i <= k; i++) {
            dp[i][0] = 0;
            dp[i][1] = -prices[0];
        }
        for (int i = 1; i < length; i++) {
            for (int j = k; j > 0; j--) {
                dp[j][0] = Math.max(dp[j][0], dp[j][1] + prices[i]);
                dp[j][1] = Math.max(dp[j][1], dp[j - 1][0] - prices[i]);
            }
        }
        return dp[k][0];
    }

    public int maxProfit(int[] prices) {
        if (prices == null || prices.length == 0) {
            return 0;
        }
        int profit0 = 0, profit1 = -prices[0];
        int length = prices.length;
        for (int i = 1; i < length; i++) {
            int newProfit0 = Math.max(profit0, profit1 + prices[i]);
            int newProfit1 = Math.max(profit1, profit0 - prices[i]);
            profit0 = newProfit0;
            profit1 = newProfit1;
        }
        return profit0;
    }
}
```



### 309. 最佳买卖股票时机含冷冻期

情况五：k 为正无穷但有冷却时间

情况五对应的题目是「309. 最佳买卖股票时机含冷冻期」。

由于具有相同的 k 值，因此情况五和情况二非常相似，不同之处在于情况五有「冷却时间」的限制，因此需要对状态转移方程进行一些修改。

情况二的状态转移方程如下：

`T[i][k][0] = max(T[i - 1][k][0], T[i - 1][k][1] + prices[i])`
`T[i][k][1] = max(T[i - 1][k][1], T[i - 1][k][0] - prices[i])`

但是在有「冷却时间」的情况下，如果在第 i - 1 天卖出了股票，就不能在第 i 天买入股票。因此，如果要在第 i 天买入股票，第二个状态转移方程中就不能使用 `T[i - 1][k][0]`，而应该使用 `T[i - 2][k][0]`。状态转移方程中的别的项保持不变，新的状态转移方程如下：

`T[i][k][0] = max(T[i - 1][k][0], T[i - 1][k][1] + prices[i])`
`T[i][k][1] = max(T[i - 1][k][1], T[i - 2][k][0] - prices[i])`

根据上述状态转移方程，可以写出时间复杂度为 O(n) 和空间复杂度为 O(n)的解法。

```
#Python 时间O(n) 空间O(n)
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        n = len(prices)
        if n == 0: return 0
        dp = [[0 for i in range(2)] for j in range(n)]
        dp[0][0] = 0
        dp[0][1] = -prices[0]
        for i in range(1, n):
            dp[i][0] = max(dp[i - 1][0], dp[i - 1][1] + prices[i])
            dp[i][1] = max(dp[i - 1][1], (dp[i - 2][0] if i >=2 else 0) - prices[i])
        return dp[n - 1][0]
```

```
#Java 时间O(n) 空间O(n)
class Solution {
    public int maxProfit(int[] prices) {
        if (prices == null || prices.length == 0) {
            return 0;
        }
        int length = prices.length;
        int[][] dp = new int[length][2];
        dp[0][0] = 0;
        dp[0][1] = -prices[0];
        for (int i = 1; i < length; i++) {
            dp[i][0] = Math.max(dp[i - 1][0], dp[i - 1][1] + prices[i]);
            dp[i][1] = Math.max(dp[i - 1][1], (i >= 2 ? dp[i - 2][0] : 0) - prices[i]);
        }
        return dp[length - 1][0];
    }
}
```

如果注意到第 `i` 天的最大收益只和第 `i - 1` 天和第 `i - 2` 天的最大收益相关，空间复杂度可以降到 O(1)。

```
#Python 时间O(n) 空间O(1)
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        n = len(prices)
        if n == 0: return 0
        prevProfit0 = 0
        profit0 = 0
        profit1 = -prices[0]
        for i in range(n):
            nextProfit0 = max(profit0, profit1 + prices[i])
            nextProfit1 = max(profit1, prevProfit0 - prices[i])
            prevProfit0 = profit0
            profit0 = nextProfit0
            profit1 = nextProfit1
        return profit0
```

```
#Java 时间O(n) 空间O(1)
class Solution {
    public int maxProfit(int[] prices) {
        if (prices == null || prices.length == 0) {
            return 0;
        }
        int prevProfit0 = 0, profit0 = 0, profit1 = -prices[0];
        int length = prices.length;
        for (int i = 1; i < length; i++) {
            int nextProfit0 = Math.max(profit0, profit1 + prices[i]);
            int nextProfit1 = Math.max(profit1, prevProfit0 - prices[i]);
            prevProfit0 = profit0;
            profit0 = nextProfit0;
            profit1 = nextProfit1;
        }
        return profit0;
    }
}
```

### 714.买卖股票的最佳时机含手续费

情况六：k 为正无穷但有手续费

情况六对应的题目是「714. 买卖股票的最佳时机含手续费」。

由于具有相同的 k 值，因此情况六和情况二非常相似，不同之处在于情况六有「手续费」，因此需要对状态转移方程进行一些修改。

情况二的状态转移方程如下：

`T[i][k][0] = max(T[i - 1][k][0], T[i - 1][k][1] + prices[i])`
`T[i][k][1] = max(T[i - 1][k][1], T[i - 1][k][0] - prices[i])`

由于需要对每次交易付手续费，因此在每次买入或卖出股票之后的收益需要扣除手续费，新的状态转移方程有两种表示方法。

第一种表示方法，在每次买入股票时扣除手续费：

`T[i][k][0] = max(T[i - 1][k][0], T[i - 1][k][1] + prices[i])`
`T[i][k][1] = max(T[i - 1][k][1], T[i - 1][k][0] - prices[i] - fee)`

第二种表示方法，在每次卖出股票时扣除手续费：

`T[i][k][0] = max(T[i - 1][k][0], T[i - 1][k][1] + prices[i] - fee)`
`T[i][k][1] = max(T[i - 1][k][1], T[i - 1][k][0] - prices[i])`

根据上述状态转移方程，可以写出时间复杂度为 O(n)和空间复杂度为 O(n)的解法。

```
#Python 时间O(n) 空间O(n)
#写法一
class Solution:
    def maxProfit(self, prices: List[int], fee: int) -> int:
        n = len(prices)
        if n == 0: return 0
        dp = [[0 for i in range(2)] for j in range(n)]
        dp[0][0] = 0
        dp[0][1] = -prices[0]
        for i in range(1, n):
            dp[i][0] = max(dp[i - 1][0], dp[i - 1][1] + prices[i] - fee)
            dp[i][1] = max(dp[i - 1][1], dp[i - 1][0] - prices[i])
        return dp[n - 1][0]
 #写法二
 class Solution:
    def maxProfit(self, prices: List[int], fee: int) -> int:
        n = len(prices)
        if n == 0: return 0
        dp = [[0 for i in range(2)] for j in range(n)]
        dp[0][0] = 0
        dp[0][1] = -prices[0] - fee
        for i in range(1, n):
            dp[i][0] = max(dp[i - 1][0], dp[i - 1][1] + prices[i])
            dp[i][1] = max(dp[i - 1][1], dp[i - 1][0] - prices[i] - fee)
        return dp[n - 1][0]
```

```
#Java 时间O(n) 空间O(n)
#写法一
class Solution {
    public int maxProfit(int[] prices, int fee) {
        if (prices == null || prices.length == 0) {
            return 0;
        }
        int length = prices.length;
        int[][] dp = new int[length][2];
        dp[0][0] = 0;
        dp[0][1] = -prices[0];
        for (int i = 1; i < length; i++) {
            dp[i][0] = Math.max(dp[i - 1][0], dp[i - 1][1] + prices[i] - fee);
            dp[i][1] = Math.max(dp[i - 1][1], dp[i - 1][0] - prices[i]);
        }
        return dp[length - 1][0];
    }
}
#写法二
class Solution {
    public int maxProfit(int[] prices, int fee) {
        if (prices == null || prices.length == 0) {
            return 0;
        }
        int length = prices.length;
        int[][] dp = new int[length][2];
        dp[0][0] = 0;
        dp[0][1] = -prices[0] - fee;
        for (int i = 1; i < length; i++) {
            dp[i][0] = Math.max(dp[i - 1][0], dp[i - 1][1] + prices[i]);
            dp[i][1] = Math.max(dp[i - 1][1], dp[i - 1][0] - prices[i] - fee);
        }
        return dp[length - 1][0];
    }
}
```

如果注意到第 `i` 天的最大收益只和第 `i - 1` 天的最大收益相关，空间复杂度可以降到 O(1)。

```
#Python 时间O(n) 空间O(1)
#写法一
class Solution:
    def maxProfit(self, prices: List[int], fee: int) -> int:
        n = len(prices)
        if n == 0: return 0
        profit0 = 0
        profit1 = -prices[0] - fee
        for i in range(1, n):
            newProfit0 = max(profit0, profit1 + prices[i])
            newProfit1 = max(profit1, profit0 - prices[i] - fee)
            profit0 = newProfit0
            profit1 = newProfit1
        return profit0
        
#写法二
class Solution:
    def maxProfit(self, prices: List[int], fee: int) -> int:
        n = len(prices)
        if n == 0: return 0
        profit0 = 0
        profit1 = -prices[0]
        for i in range(1, n):
            newProfit0 = max(profit0, profit1 + prices[i] - fee)
            newProfit1 = max(profit1, profit0 - prices[i])
            profit0 = newProfit0
            profit1 = newProfit1
        return profit0

```

```
#Java 时间O(n) 空间O(1)
#写法一
class Solution {
    public int maxProfit(int[] prices) {
        if (prices == null || prices.length == 0) {
            return 0;
        }
        int profit0 = 0, profit1 = -prices[0] - fee;
        int length = prices.length;
        for (int i = 1; i < length; i++) {
            int newProfit0 = Math.max(profit0, profit1 + prices[i]);
            int newProfit1 = Math.max(profit1, profit0 - prices[i] - fee);
            profit0 = newProfit0;
            profit1 = newProfit1;
        }
        return profit0;
    }
}

#写法二
class Solution {
    public int maxProfit(int[] prices) {
        if (prices == null || prices.length == 0) {
            return 0;
        }
        int profit0 = 0, profit1 = -prices[0];
        int length = prices.length;
        for (int i = 1; i < length; i++) {
            int newProfit0 = Math.max(profit0, profit1 + prices[i] - fee);
            int newProfit1 = Math.max(profit1, profit0 - prices[i]);
            profit0 = newProfit0;
            profit1 = newProfit1;
        }
        return profit0;
    }
}
```

