# Pattern: Breadth First Search, 广度优先搜索

## **类别总结:**
广度优先算法 BFS 是一类搜索算法大的框架, 题型涉及到对树的搜索操作, 对图graph的搜索遍历, 以及诸如走迷宫,岛数,字母阶梯等的应用问题. 

涵盖的思路是以联通某层节点的单个层进行搜索遍历, 完成后再进入下一层. 第一次刷题总结, 我们针对在树上的BFS (Tree Search) 这类基础题型, 先行实践BFS算法的特性和代码套路. 后续深入学校再进行其他高频题型 (主要是应用题: 迷宫, 文字楼梯, 岛数等) 的练习. 同时, 当中高频应用题型, 基本全可用DFS来解决 (甚至更需要使用DFS思路). 

## 1. 树上的BFS
这种模式基于广度优先搜索（Breadth First Search (BFS)），适用于需要遍历一颗树。借助于**队列**数据结构，从而能保证树的节点按照他们的层数打印出来。打印完当前层所有元素，才能执行到下一层。这类题型有很强的模板套路性, 往往题干要求就是求 按层遍历一棵树, 求一棵树的高度等等. 

这种树上的BFS模式是通过把根节点加到队列中，然后不断遍历直到队列为空。每一次循环中，我们都会把队头结点拿出来（remove），然后对其进行必要的操作。在删除每个节点的同时，其孩子节点，都会被加到队列中. 详细流程请参考代码模板, 以及下方有趣的动图!

<img src="https://upload-images.jianshu.io/upload_images/10107787-b9da64a7c631e0e6.gif?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp" />

## **经典题目:**

**Tree BFS: 针对树的广度优先算法题**

- 102. Binary tree level order traversal (med)
- 103. Binary tree zigzag level order traversal (med)
- 107. Binary tree level order traversal (bottom-up) (easy)
- 429. N-ary tree level order traversal (med)

- 111. Minimum depth of binary tree (easy)
- 513. Find bottom left tree value (med)

**The Maze: 迷宫题**

- 490 505 499

**BFS 应用及其它树的高频题**

- 127 130 199 200 207 279 286 310 863

## **参考链接 Reference:**

- https://www.jianshu.com/p/725de069938c (最好的BFS DFS详解)
- https://www.cnblogs.com/zhangwanying/p/9706908.html (leetcode BFS题目汇总)
- https://shawnlyu.com/algorithms/intro-to-graph-algorithms-bfs-dfs/

## **模板 Template:**
### **Python**
```py
def BFSTreeSearch (root):
  # Error check for empty tree
  if not root:
    return None

  # Initialize queue to record nodes by layers
  queue = [root]

  # While-loop over queue until no more node
  # In this template, we loop over entire layer at a single while loop
  while queue:
    levelSize = len(queue)
    # loop over all nodes in the current layer
    for i in range(levelSize):
      # take out one node from the queue
      node = queue.pop(0)
      # put in all children of this node to the queue
      if node.left: queue.append(node.left)
      if node.right: queue.append(node.right)

  # Return something: we can follow this template and use some variables to record the height or node value layer by layer
  return result
```
### **JavaScript**
```js
const BFSTreeSearch = (root) => {
  // Error check for empty tree
  if (!root)
    return null;

  // Initialize queue to record nodes by layers
  const queue = [root];

  // While-loop over queue until no more node
  // In this template, we loop over entire layer at a single while loop
  while (queue.length) {
    const levelSize = queue.length;
    // loop over all notes in the current layer
    for (let i = 0; i < levelSize; i++) {
      // take out one node from the queue
      const node = queue.shift();
      // put in all children of this node to the queue
      if (node.left)
        queue.push(node.left);
      if (node.right)
        queue.push(node.right);
    }
  }

  // Return something: we can follow this template and use some variables to record the height or node value layer by layer
  return result;
}
```

Or we don't need to specify the single layer if we don't care about the tree height or width
```js
const BFSTreeSearch = (root) => {
  // Error check for empty tree
  if (!root)
    return null;

  // Initialize queue to record nodes by layers
  const queue = [root];

  // While-loop over queue until no more node
  while (queue.length) {
    // take out one node from the queue
    const node = queue.shift();
    // put in all children of this node to the queue
    if (node.left)
      queue.push(node.left);
    if (node.right)
      queue.push(node.right);
  }
}
```
## **BFS Graph 模板 Template:**
### **Python**
```py
def BFSGraphSearch (relationship, num, root):
  # Construct the graph.
  graph = dict()
  for node in num:
    graph[node] = []
  for (start, end) in relationship:
    graph[start].append(end)
  # Initialize queue to record nodes by layers
  queue = [root]

  # While-loop over queue until no more node
  # In this template, we loop over entire layer at a single while loop
  while queue:
    levelSize = len(queue)
    # loop over all nodes in the current layer
    for i in range(levelSize):
      # take out one node from the queue
      node = queue.pop(0)
      # put in all neighbors of this node to the queue
      for neighbor in graph[node]:
        # Add the connected node to the queue
        queue.push(neighbor)

  # Return something: we can follow this template and use some variables to record the height or node value layer by layer
  return result
```

## **Best First Search (Dijsktra's Algorithm) Graph 模板 Template:**
### **Python**
```py
import heapq
def BFSGraphSearch (relationship, num, root, [optional] destination):
  # Construct the graph and initialize the visited map (costs)
  graph = dict()
  costs = dict()
  for node in num:
    graph[node] = []
  for (start, end, cost) in relationship:
    graph[start].append((end, cost))

  # Initialize heap to record cost and nodes
  # heapq won't instantiate a new heap object, but works on list (static methods)
  # heapq only works on min heap. If want to create max heap, negatively reverse all numbers. Put cost ahead of the node.
  minheap = [(0, root)]

  # While-loop over heap until no more node
  # In this template, we loop over entire layer at a single while loop
  while minheap:
    # Pop the optimal current node to visit, top = heapq.heappop(minheap)
    node = heapq.heappop(minheap)
    # If visited before, skip it
    if (node[1] in costs):
      continue
    # Update the cost of the current node
    costs[node[1]] = node[0]
    # If this is the destination, return the cost
    if node[1] == destination:
      return costs[node[1]]
    # loop over all connected nodes of this node, and add them to the heap.
    for (neighbor, cost) in graph[node]:
      # Add the connected node and the cost to reach this node to the heap, heapq.heappush(minheap, item)
      heapq.heappush(minheap, (node[0] + cost, neighbor))

  # Return something: most likely maximum costs 
  return max(costs.values())
```
## **Topological Sort Graph 模板 Template:**
### **Python**
```py
def BFSGraphSearch (relationship, num, root, [optional] destination):
  # Construct the graph and initialize the visited map (costs)
  graph = dict()
  indegree = dict()
  for node in num:
    graph[node] = []
    indegree[item] = 0
  for (start, end, cost) in relationship:
    graph[start].append(end)
    indegree[end] += 1

  # Initialize queue to record nodes by layers
  queue = list()
  # Initialize queue with all node with indegree value 0
  for key in indegree:
    if indegree[key] == 0:
      queue.append(key)
  # Now we can do BFS search until queue empty
  # we may also step up result variables such as 'path' or 'count'
  path = [] 
  while queue:
    ## BFS: queue out: 
    # take out the current node and update result
    node = queue.pop(0)
    path.append(node)
    ## BFS: queue in: 
    # for the current node, find its all adjacent neighbors to whom the current node is pointing
    for neighbor in graph[node]:
      indegree[neighbor] -= 1
      # if find new zero-in-degree node, we push it to the queue
      if indegree[neighbor] == 0:
        queue.append(neighbor)

  ## Step 4: usually the final result should have the same length of all items
  if len(path) == num:
    return True
  else:
    return False
```

