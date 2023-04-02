# 最少航班—广度优先搜索样本问题及解决方案

> 原文：<https://javascript.plainenglish.io/fewest-flights-a-breadth-first-search-sample-problem-and-solution-72c5224c71af?source=collection_archive---------5----------------------->

![](img/da120cca2992884ac0af895d71b1b9ff.png)

这是一个广度优先搜索的示例问题。为了快速阅读这是什么，以及对图形的基本理解，请阅读[这个](https://medium.com/javascript-in-plain-english/data-structures-understanding-graphs-82509d35e6b5)。

**问题陈述:**有 **n** 个城市通过 **m** 个航班连接。每次战斗从城市 **u** 出发，到达城市 **v** 。

现在给定所有的城市和航班，以及起始城市 **src** 和目的地 **dst** ，任务是找出从 **src** 到 **dst** 所需的最少航班数。如果没有这条路线，输出 **-1** ，如果 **src** 和 **dst** 相同，输出 **0** 。

对于给定的输入:

> src--> " ATL " dest--> " NYC "航班-->[["ATL "，" NYC"]，["NYC "，" LA"]，[" ATL "，" MIA"]]

输出:

```
const fewestFlights = function(src, dest, flights) {
    const graph = toAdjacencyList(flights);
    const queue = [];
    const visited = new Set();
    visited.add(src);
    queue.push(src);
```

使用广度优先搜索找到从 src(源)到 dest(目的地)的最短航班数。我们希望使用 numberOfFlights 来存储到达每个位置需要多少次飞行。

首先，我们需要写一个函数，它接受一个边列表，并返回一个邻接表。

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

步骤 1:创建一个名为 numberOfFlights 的对象来跟踪到达一个城市的航班次数。关键是城市，价值是到达那个城市的航班数。

`const numberOfFlights = {};`

第二步。将 numberOfFlights[src]设置为 0。由于我们从 **src** 出发，所以只需 0 次飞行即可到达:

`numberOfFlights[src] = 0;`

```
while (queue.length > 0) {
      const currentCity = queue.shift();
      const adjacentCities = graph[currentCity] || [];
```

第三步。如果您到达目的地，返回到达目的地所需的航班号:

```
if (currentCity === dest) {
        return numberOfFlights[dest];
    }
```

第四步。到达下一个城市的航班数量等于`numberOfFlights`到达当前城市+1

```
for (nextCity of adjacentCities) {
        if (!visited.has(nextCity)) { numberOfFlights[nextCity] = numberOfFlights[currentCity] + 1; queue.push(nextCity);
            visited.add(nextCity);
        }
}
}
```

如果没有从 src 到 dest 的路径，则返回-1

```
return -1;
}
```

你可以在[这里](https://codepen.io/rachelhawa/pen/xxxowEM)查看上面正在使用的代码和测试用例。