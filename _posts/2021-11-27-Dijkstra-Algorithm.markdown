---
layout: post
title:  "狄杰斯特拉算法的实现"
cover-img: 
thumbnail-img:
share-img: 
date:   2021-11-27 10:16:26 +0800
tags: [python, Record]
---
**广度优先搜索**找出的是段数最少的路径，但是事实上这可能不是最快的路径，狄杰斯特拉算法提供了解决问题的办法。

该算法包含以下四个步骤：

1.找出在最短时间内达到的节点；

2.更新该节点的所有开销；

3.重复1，2 过程，历遍所有节点。

4.计算最终路径。

如果该节点无法直接到达终点，那么该节点到终点的距离为无穷大。我们首先找到距离起点最近的节点，计算到达该节点的最短时间；再找到所有与起点相连的节点，并计算它们与起点距离的时间；将这些点全部加入一个列表中[1]。

在列表[1]中的所有的节点中找到一个距离起点最短的节点，以这个节点为象征起点并加上它与起点之间的权重。查看这个象征起点是否有可以达到**以前不能达到的节点**，如果有，就找到与这个象征节点相连的所有节点，继续将它们加入列表[1]中；如果没有，寻找列表中距离起点第二短的节点，把它视为象征起点，加上它与起点之间的权重，查看这个象征起点是否有可以达到**以前不能达到的节点**，重复以上步骤。

直到找到的这个**以前不能达到的节点**就是终点，那么该流程已经结束我们已经找到了最短的路径。

Python代码如下：

```
def find_lowest_cost_node(costs): 
    lowest_cost = float("inf")
    lowest_cost_node = None
    for node in costa:
        cost = costs(node)
        if cost < lowest_cost and node not in processed:
            lowest_cost = cost
            lowest_cost_node = node
    return lowest_cost_node
node = find_lowest_cost_node(costs)
while node is not None:
    cost = costs(node)
    neighbors = graph[node]
    for n in neighbors.keys():
        new_cost = cost + neighbors[n]
        if costs[n] > new_cost:
            costs[n] = new_cost
            parents[n] = node
    processed.append(node)
    node = find_lowest_cost_node(costs)
```

广度优先搜索用于在非加权图中查找最短路径。

狄杰斯特拉算法用于在加权图中查找最短路径。

如果权重为负时，狄杰斯特拉算法无法使用。(可以参考贝尔曼-福德算法)
