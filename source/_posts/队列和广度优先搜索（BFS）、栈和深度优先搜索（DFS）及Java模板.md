---
title: 队列和广度优先搜索（BFS）、栈和深度优先搜索（DFS）及Java模板
date: 2019-11-09 22:00:00
author: Ziming M
top: true
cover: true
summary: 本文介绍了BFS和DFS原理，并提供相关Java模板
categories: 算法
tags:
  - Python
  - 算法
  - BFS
  - DFS
---
本文为[Leetcode]([https://leetcode-cn.com/explore/learn/card/queue-stack/217/queue-and-bfs/870/](https://leetcode-cn.com/explore/learn/card/queue-stack/217/queue-and-bfs/870/)
)学习笔记
## 队列和广度优先搜索（BFS）
广度优先搜索（BFS）的一个常见应用是找出从根结点到目标结点的最短路径。在本文中，我们提供了一个示例来解释在 BFS 算法中是如何逐步应用队列的。

##### 1. 结点的处理顺序是什么？

在第一轮中，我们处理根结点。在第二轮中，我们处理根结点旁边的结点；在第三轮中，我们处理距根结点两步的结点；等等等等。

与树的层序遍历类似，越是接近根结点的结点将越早地遍历。

如果在第 k 轮中将结点 X 添加到队列中，则根结点与 X 之间的最短路径的长度恰好是 k。也就是说，第一次找到目标结点时，你已经处于最短路径中。

##### 2. 队列的入队和出队顺序是什么？

我们首先将根结点排入队列。然后在每一轮中，我们逐个处理已经在队列中的结点，并将所有邻居添加到队列中。值得注意的是，新添加的节点不会立即遍历，而是在下一轮中处理。

结点的处理顺序与它们添加到队列的顺序是完全相同的顺序，即先进先出（FIFO）。这就是我们在 BFS 中使用队列的原因。

> 在特定问题中执行 BFS 之前确定结点和边缘非常重要。通常，结点将是实际结点或是状态，而边缘将是实际边缘或可能的转换。

##### *模板 I*

```
/**
 * Return the length of the shortest path between root and target node.
 */
int BFS(Node root, Node target) {
    Queue<Node> queue;  // store all nodes which are waiting to be processed
    int step = 0;       // number of steps neeeded from root to current node
    // initialize
    add root to queue;
    // BFS
    while (queue is not empty) {
        step = step + 1;
        // iterate the nodes which are already in the queue
        int size = queue.size();
        for (int i = 0; i < size; ++i) {
            Node cur = the first node in queue;
            return step if cur is target;
            for (Node next : the neighbors of cur) {
                add next to queue;
            }
            remove the first node from queue;
        }
    }
    return -1;          // there is no path from root to target
}
```
1. 如代码所示，在每一轮中，队列中的结点是等待处理的结点。
2. 在每个更外一层的 while 循环之后，我们距离根结点更远一步。变量 step 指示从根结点到我们正在访问的当前结点的距离。

##### *模板II*

有时，确保我们永远不会访问一个结点两次很重要。否则，我们可能陷入无限循环。如果是这样，我们可以在上面的代码中添加一个哈希集来解决这个问题。这是修改后的伪代码：
```
/**
 * Return the length of the shortest path between root and target node.
 */
int BFS(Node root, Node target) {
    Queue<Node> queue;  // store all nodes which are waiting to be processed
    Set<Node> used;     // store all the used nodes
    int step = 0;       // number of steps neeeded from root to current node
    // initialize
    add root to queue;
    add root to used;
    // BFS
    while (queue is not empty) {
        step = step + 1;
        // iterate the nodes which are already in the queue
        int size = queue.size();
        for (int i = 0; i < size; ++i) {
            Node cur = the first node in queue;
            return step if cur is target;
            for (Node next : the neighbors of cur) {
                if (next is not in used) {
                    add next to queue;
                    add next to used;
                }
            }
            remove the first node from queue;
        }
    }
    return -1;          // there is no path from root to target
}
```

> 有两种情况你不需要使用哈希集：
> 1. 你完全确定没有循环，例如，在树遍历中；
> 2. 你确实希望多次将结点添加到队列中。

## 栈和深度优先搜索（DFS）

与 BFS 类似，**深度优先搜索**（DFS）也可用于查找从根结点到目标结点的路径。在本文中，我们提供了示例来解释 DFS 是如何工作的以及栈是如何逐步帮助 DFS 工作的。
[图](https://leetcode-cn.com/explore/learn/card/queue-stack/219/stack-and-dfs/881/)

##### 1. 结点的处理顺序是什么？

在上面的例子中，我们从根结点 A 开始。首先，我们选择结点 B 的路径，并进行回溯，直到我们到达结点 E，我们无法更进一步深入。然后我们回溯到 A 并选择第二条路径到结点 C 。从 C 开始，我们尝试第一条路径到 E 但是 E 已被访问过。所以我们回到 C 并尝试从另一条路径到 F。最后，我们找到了 G。

总的来说，在我们到达最深的结点之后，我们只会回溯并尝试另一条路径。
> 因此，你在 DFS 中找到的第一条路径并不总是最短的路径。例如，在上面的例子中，我们成功找出了路径 A-> C-> F-> G 并停止了 DFS。但这不是从 A 到 G 的最短路径。

##### 2. 栈的入栈和退栈顺序是什么？

我们首先将根结点推入到栈中；然后我们尝试第一个邻居 B 并将结点 B 推入到栈中；等等等等。当我们到达最深的结点 E 时，我们需要回溯。当我们回溯时，我们将从栈中弹出最深的结点，这实际上是推入到栈中的最后一个结点。

结点的处理顺序是完全相反的顺序，就像它们被添加到栈中一样，它是后进先出（LIFO）。这就是我们在 DFS 中使用栈的原因。

正如我们在本章的描述中提到的，在大多数情况下，我们在能使用 BFS 时也可以使用 DFS。但是有一个重要的区别：**遍历顺序**。

与 BFS 不同，**更早访问的结点可能不是更靠近根结点的结点**。因此，你在 DFS 中找到的第一条路径**可能不是最短路径**。
##### *模板 - 递归*
```
/*
 * Return true if there is a path from cur to target.
 */
boolean DFS(Node cur, Node target, Set<Node> visited) {
    return true if cur is target;
    for (next : each neighbor of cur) {
        if (next is not in visited) {
            add next to visted;
            return true if DFS(next, target, visited) == true;
        }
    }
    return false;
}
```
当我们递归地实现 DFS 时，似乎不需要使用任何栈。但实际上，我们使用的是由系统提供的隐式栈，也称为调用栈。
##### *示例*
让我们看一个例子。我们希望在下图中找到结点 0 和结点 3 之间的路径。我们还会在每次调用期间显示栈的状态。
![Stack Status](https://upload-images.jianshu.io/upload_images/19942712-20b7c9e726b380f2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
在每个堆栈元素中，都有一个整数 **cur**，一个整数 **target**，一个对**访问过的**数组的引用和一个对数组**边界**的引用，这些正是我们在 DFS 函数中的参数。我们只在上面的栈中显示 **cur**。

每个元素都需要固定的空间。栈的大小正好是 DFS 的深度。因此，在最坏的情况下，维护系统栈需要 O(h)，其中 h 是 DFS 的最大深度。在计算空间复杂度时，永远不要忘记考虑系统栈。
> 在上面的模板中，我们在找到第一条路径时停止。
> 如果你想找到最短路径呢？
> 提示：再添加一个参数来指示你已经找到的最短路径。