# 查找路径—深度优先搜索

> 原文：<https://javascript.plainenglish.io/find-path-depth-first-search-28d27bba8ae0?source=collection_archive---------5----------------------->

![](img/e03c94f913ee74a82cc8c727c8f6bdca.png)

这是深度优先搜索的一个样例问题。要快速阅读这是什么，以及对图形的基本理解，请阅读[这个](https://medium.com/javascript-in-plain-english/data-structures-understanding-graphs-82509d35e6b5)。

**问题陈述:**

给定一个图，一个起点( **src** )和一个目的地( **dest** )，返回从 **src** 到 **dest** 的路径，如果没有路径，返回一个空数组。

给定以下边缘列表:

```
let smallFlightMap = [
  ["ATL", "DC"],
  ["DC", "NYC"],
  ["ATL", "DAL"],
  ["DAL", "DC"]
]
```

首先，编写一个转换为邻接表的函数:

```
const toAdjacencyList = function(edgeList) {
    const adjacencyList = {};
    for (let edge of edgeList) {
      const src = edge[0];
      const dest = edge[1]; if (adjacencyList[src] != undefined) {
          adjacencyList[src].push(dest);
      } else {
          adjacencyList[src] = [dest];
      }
    }
    return adjacencyList;
}
```

目标是将所有片段写入以下函数，该函数使用深度优先搜索找到从 **src** (源)到 **dest** (目的地)的路径:

```
const findPath = function(src, dest, flights) {
    const graph = toAdjacencyList(flights);
    const visited = new Set();
    const stack = [];
    stack.push(src);
```

**步骤 1。创建一个名为`cameFrom`的对象来记录你从哪里来。城市将是键，值是前一个城市，我们称之为`previousCity`**

```
const cameFrom = {};
```

**步骤二。**将`cameFrom[src]`设置为未定义。因为你是从 src 开始的，它不是从任何地方来的。

```
cameFrom[src] = undefined;while (stack.length > 0) {
    const currentCity = stack.pop();
    console.log(currentCity);
    const adjacentCities = graph[currentCity] || [];
```

注意，在最后一行，如果`graph[currentCity]`未定义，它将返回一个空数组。

**步骤三。**如果到达目的地，通过`cameFrom`返回，建立从`src`到`dest`的路径。

您将需要一个变量— `pathFromSrcToDes` t，以一个空数组开始，并使用 unshift()将目的地添加到数组的前面。`const pathFromSrcToDest = []`和`pathFromSrcToDest.unshift(dest)`

接下来，创建另一个变量来跟踪 path (stack)中的前一个城市，该变量被设置为您刚来的目的地。，`let previousCity = cameFrom[dest]`。

```
if (currentCity === dest) {
        const pathFromSrcToDest = [];
        pathFromSrcToDest.unshift(dest);
        let previousCity = cameFrom[dest];
                }
```

使用`unshift()`将`previousCity` 添加到`pathFromSrcToDest`并设置`previousCity = cameFrom[previousCity]`然后返回`pathFromSrcToDest`。

```
while (previousCity != undefined) {
          pathFromSrcToDest.unshift(previousCity);
          previousCity = cameFrom[previousCity];
        }
        return pathFromSrcToDest;
      }
```

设置`cameFrom[nextCity] = currentCity`并记住如果解未定义(没有路径)则返回一个空数组。

```
for (nextCity of adjacentCities) {
        if (!visited.has(nextCity)) 
         cameFrom[nextCity] = currentCity;
         stack.push(nextCity);
         visited.add(nextCity);
            }
     }
 }
 return [];
}
```

你可以在[这里](https://codepen.io/rachelhawa/pen/XWJzxbj)查看上面正在使用的代码和测试用例。