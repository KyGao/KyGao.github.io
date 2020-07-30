 ---
title: 极客时间-数据结构与算法
date: 2020-07-24 10:50:43
tags: [课内,大三上,C方向]
categories: 
- leetcode
---

### 数组&链表
1. 数组：
  - Access: O(1)
  - Insert: 平均O(n)
  - Delete: 平均O(n)
2. 链表：
  - space: O(n)
  - prepend: O(1)
  - append: O(1)
  - lookup: O(n)
  - insert: O(1)
  - delete: O(1)
3. 链表题思考的逻辑都很简单，难就难在代码实现
4. **典型例题**：链表反转，两两交换，环形链表

### 堆栈&队列 (Stack&Queue)
1. Stack - FILO
2. Queue - FIFO
3. http://www.bigocheatsheet.com
4. **典型例题**：括号匹配，互相实现(不太可能遇到因为没啥用)
5. 判断stack为空用not stack而不是stack is None
6. 字典的调用用中括号

### 优先队列 (PriorityQueue)
1. 优先级：最大/最小/出现次数/...
2. 一般面试时候问你知不知道，不会让你去实现 => 要了解实现机制
    1. Heap(Binary, Binominal, Fibonacci)（堆）
    2. Binary Search Tree（二叉搜索树）
3. 小顶堆/大顶堆现在用处很少了
4. 堆：二叉堆性能比较差，基本上所有堆找到最小/最大元素都是O(1)，关键是合并，插入和删除的维护上的时间复杂度，就作为堆的指标。斐波那契/严格斐波那契堆性能最好
5. **典型例题**：数据流中第k大元素，滑动窗口最大值
6. heapq.heappushpop弹出的是最小值而不是滑动窗口最前面的值
7. heapq可能用到的方法：`heapq.heappush(heap, item)`  `heapq.heappop(heap)` `heapq.heappushpop(heap, item)` `heapq.heapify(x)` `heapq.nlargest(n, iterable)`
8. 即使是一维数组，enumerate也是返回两个值  `for index, val in enumerate(list)`

### 映射&集合 (Map&Set)
1. Hash Function 哈希函数：原本的线性查找是O(n)，希望能O(1)的复杂度进行查找 => 对key做变换为一个index (求和取模...)
2. Hash Collision 最常见的处理办法：拉链法，即某个地址拉出一个链表，满足的都挂上来
3. List vs. Map (就是字典) vs. Set (Set比List效率更高)
4. Map的实现方式有两种（Set同理）：HashMap vs. TreeMap (hashtable vs. binary-search-tree) (查找：O(1) vs. O(log2N) but tree的方式是有序的)
5. **典型例题**：（和查询计数有关都可以）字母异位词，两数之和，三数之和
6. Set的常用方法：
  - 增加：加一个`thisset.add("orange")`  加多个`thisset.update(["orange", "mango", "grapes"])`
  - 查看是否存在：`if "banana" in thisset:`
  - 删除：没有会报错`thisset.remove("banana")`  没有不报错`thisset.discard("banana")`
  - 构造一个set：`set(("apple", "banana", "cherry"))`
7. dict / Map的常用方法：
  - 调用（根据key获取val）：`x = thisdict["model"]``x = thisdict.get("model")`但是用get可以加参数，如果不存在该key-val，就新建一个。如`x = thisdict.get("model", 0)`
  - 查看是否存在：`if "model" in thisdict:`
  - 删除：`thisdict.pop("model")`与这个一样`del thisdict["model"]`

### 树&二叉树&二叉搜索树
1. Linked List 就是特殊化的 Tree， Tree 就是特殊化的 Graph
2. 二叉搜索树，也称有序二叉树(order)，排序二叉树(sorted), 是指一棵空树或者具有下列性质的二叉树：
  1. 若任意节点的左子树不为空，则左子树上所有节点的值均小于他的根节点的值；
  2. 若任意节点的右子树不为空，则右子树上所有节点的值均大于他的根节点的值；
  3. 任意节点的左、右子树也分别为二叉查找树
  - 注意是子树！！！不是儿子！！！之前一直理解错了
3. **典型例题**：验证二叉搜索树，LCA
4. 二叉搜索树与中序遍历天然联系：二叉搜索树的中序遍历是递增数组
5. 二叉搜索树不能出现相等的值！！
6. Python3开始用 `sys.maxsize`表示最大值常量

### 递归&分治 (Recursion&Divide_and_Concur)
1. 递归
  ```python
  def recursion(level, param1, param2, ...):
    # recursion terminator
    if level > MAX_LEVEL:
        print_result
        return

    # process logic in current level
    process_data(level, data, ...)

    # drill down
    self.recursion(level + 1,  p1, ...)

    # reverse the current level status if needed
    reverse_state(level)
  ```
2. 分治
```python
```python
  def divide_conquer(problem, param1, param2, ...):
    # recursion terminator
    if problem is None:
        print_result
        return

    # process data
    data = prepare_data(problem)
    subproblems = split_problem(problem, data)

    # concur subproblem
    subresult1 = self.divide_conquer(subproblem[0], p1, ...)
    subresult2 = self.divide_conquer(subproblem[1], p1, ...)
    subresult3 = self.divide_conquer(subproblem[2], p1, ...)

    # process and generate the final result
    result = process_result(subresult1, subresult2, subresult3, ...)
```
3. **典型例题**:Pow(x, n)

### 贪心算法
1. 适用贪心算法的场景必须是"子问题的最优解能递推到最终问题的最优解"
2. 贪心算法与动态规划的不同在于它对每个子问题的解决方案都做出选择,不能回退.动态规划则会保存以前的运算结果,并根据以前的结果对当前进行选择,可以回退.
3. **典型例题**:买卖股票
4. 买卖股票系列问题:同时持有股数,买卖次数不一样

### BFS & DFS
1. BFS代码框架 (树/图都适用):
  ```python
  def BFS(self, start, end):
    queue = [start]
    visited = set(start)   # 树不会重复,所以用set主要针对图
    
    while queue:
        node = queue.pop()
        visited.add(node)   

        process(node)
        nodes = generate_related_nodes(node)
        queue.push(nodes)
   
    return visited
  ```
2. DFS代码框架 (树/图都适用) 计算机本身处理利用栈,更像DFS
  ```python
  visited = set()
  def dfs(self, node, visited):
    visited.add(node)
    
    for next_node in node.children():
        if not next_node in visited:
            dfs(next_node, visited)
  ```
  非递归
  ```python
  def DFS(self, tree):
    if not tree.root:
        return []

    visited, stack = [], [tree.root]
    
    while stack:
        node = stack.pop()
        visited.add(node)   

        process(node)
        nodes = generate_related_nodes(node)
        stack.push(nodes)
   
    return visited
  ```

### 剪枝
1. **典型例题**:N皇后,数独,生成有效括号组合

### 二分查找
1. 二分查找要求: Sorted, Bounded. Accessible by index
2. 模板
    ```python
  left, right = 0, len(array) - 1
  while left <= right:
    mid = left + (right - left) / 2
    if array[mid] == target:
      return or break
    elif array[mid] < target:
      left = mid + 1
    else:
      right = mid - 1
    ```
3. **典型例题**:sqrt

### 位运算
1. 实战常用的位运算操作:
  - `x & 1 == 1` 判断奇偶 (`x % 2 == 1`)
  - `x = x & (x - 1)`:清除最低位的1
2. **典型例题**:位1的个数,2的幂次方

### 动态规划 (DP)
1. 四点核心概念:
  1. 递归 + 记忆化 => 递推     (动态规划就是动态递推)
  2. 状态的定义: opt[n], dp[n], fib[n]  (要是数组,状态定义对了就是成功的一半)
  3. 状态转移方程: opt[n] = best_of(opt[n-1], opt[n-2], ...)
  4. 最优子结构
2. 递归法解斐波那契数列:O(2^n)
  ```c++
  int fib(int n) {
    return n <= 1 ? n : fib(n - 1) + fib(n - 2);
  }
  ```
  
  动态规划法解斐波那契数列:O(n)
  状态转移方程: F[n] = F[n-1] + F[n-2]
  ```C++
  F[0] = 0, F[1] = 1;
  for (int i = 2; i <=n; ++i) {
    # 记下每次的F[i],使之不需要重复计算
    F[i] = F[i - 1] + F[i - 2];
  }
  ```
  **递归:自上而下, 递推:自下而上**
3. 棋盘,左上->右下.最短路径可以走的可能性是 $C^n_{m+n}$
  不需要考虑石头在边上只有一种走法!因为到了石头上就变成0种走法,相当于刚刚只有一种走法!
  递归法: O(exponential)
    ```c++
    int countPaths(boolean[][] grid, int row, int col) {
      if (!validSquare(grid, row, col)) return 0;  # 到石头上
      if (isAtEnd(grid, row, col)) return 1;   # 到边缘
      return countPaths(grid, row+1, col) + countPaths(grid, row, col+1)
    }
    ```
  递推法: 状转方程: 右侧/下侧可以走的可能性之和 O(m*n) 
    ```python
    if a[i, j] == '空地':
      opt[i, j] = opt[i + 1, j] + opt[i, j + 1]
    else:
      opt[i, j] = 0
    ```
4. DP vs. 回溯 vs. 贪心
  - 回溯(递归) - 重复计算
  - 贪心 - 永远局部最优
  - DP - 记录局部最优子结构 / 多种记录值
5. **典型例题**: 爬楼梯, 股票买卖系列
6. 以股票买卖为例,掌握这个状态构建就掌握基本所有能面试到的DP了:
  ```python
  # 状态定义: MP[i]表示到第 i 天的 max profit
  # 状态方程: MP[i] = MP[i-1] + (-prices[i]) 买股票
  #          MP[i] = MP[i-1] + (+prices[i])  卖股票

  # 但是发现无法根据题意进行状态推导 
  # => 需要增加一维状态: MP[i][j], j 有0, 1, 0 表示没有股票, 只能买不能卖； 1 表示有股票, 只能卖不能买
  # => 状态方程: MP[i][0] = max(MP[i-1][0], MP[i-1][1] + a[i])   # 不动  /  卖
  #             MP[i][1] = max(MP[i-1][1], MP[i-1][0] - a[i])   # 不动  /  买

  # 这样可以解决1次/无数次的问题了, 但是因为没有记录已经交易的次数, 所以还需要增加状态 k 表示已经交易了多少次
  # => 状态空间: MP[i][k][j]    # 习把买没买放在最后
  # => 状态方程: for i: 0 -> n-1:
  #                 for k: 0 -> K:
  #                     MP[i, k, 0] = max(MP[i-1, k, 0], MP[i-1, k-1, 0] + prices[i])  # 不动 / 卖
  #                     MP[i, k, 1] = max(MP[i-1, k, 1], MP[i-1, k, 0] - prices[i])    # 不动 / 买
  #             return max{MP[0->n-1][0->k][0]}

  # 更进一步考虑,如果可以持有最多X股怎么搞
  # 就将状态 j 不仅表示有没有持有股票,而是持有几股
  # => 状态方程: for i: 0 -> n-1:
  #                 for k: 0 -> K:
  #                     for j: 0 -> X:
  #                         MP[i, k, j] = max(MP[i-1, k, j], MP[i-1, k-1, j+1] + prices[i], MP[i-1, k-1, j-1] - prices[i])  # 不动 / 卖  /  买
  ```