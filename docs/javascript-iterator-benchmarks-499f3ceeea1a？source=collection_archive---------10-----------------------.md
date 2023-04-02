# JavaScript 迭代器基准

> 原文：<https://javascript.plainenglish.io/javascript-iterator-benchmarks-499f3ceeea1a?source=collection_archive---------10----------------------->

![](img/72221e7a58ffe94a74330450953c33a6.png)

下面是 JavaScript 中各种迭代方式的基准和伴随函数。

有些人可能认为没有真实应用程序数据的基准测试是没有意义的，但是我仍然发现理解这些人工设置中的运行时含义是有用的。

每一个都通过重复**1000 万次**来测试。

**TLDR**；常规的 ***为循环*** *为* ***为最高性能*** 而 ***为最高性能*** ***为循环*** *为最低性能**。*

# *以下是排名:*

1.  ***为循环** — 8 毫秒*
2.  ***While 循环** — 8 毫秒*
3.  ***用于环内** — 16 毫秒*
4.  ***对于每个** — 18 毫秒*
5.  ***滤镜** — 34 毫秒*
6.  ***减少** — 34 毫秒*
7.  ***地图** — 69 毫秒*
8.  ***用于**的— 152 毫秒*
9.  ***递归** —错误:最大堆栈调用*

# *对于每个*

```
*let **arr** = new **Array**(10000000)function **foreachPerformance**(x){
 let t = Date.now();
 x.forEach(x=>x);
 let t2 = Date.now()
 console.log(“for each: “ , t2-t)
}**foreachPerformance(arr)** // for each:  18*
```

# *地图*

```
*let **arr** = new **Array**(10000000)function **mapPerformance**(x){
 let t = Date.now();
 x.map(x=>x);
 let t2 = Date.now()
 console.log(“map: “ , t2-t)
}**mapPerformance(arr)**
// map: 69*
```

# *过滤器*

```
*let **arr** = new **Array**(10000000)function **filterPerformance**(x){
 let t = Date.now();
 x.filter(x=>x);
 let t2 = Date.now()
 console.log(“filter: “ , t2-t)
}**filterPerformance(arr)** // filter: 34*
```

# *减少*

```
*let **arr** = new **Array**(10000000)function **reducePerformance**(x){
 let t = Date.now();
 x.reduce(x=>x ,’’);
 let t2 = Date.now()
 console.log(“reduce: “ , t2-t)
}**reducePerformance(arr)** // reduce: 34*
```

# *在…期间*

```
*let **arr** = new **Array**(10000000)function **whilePerformance**(x){
 let t = Date.now();
 let i = 0;
 while(i < x.length){
    i++
 }
 let t2 = Date.now()
 console.log(“while: “ , t2-t)
}**whilePerformance(arr)
// while: 8***
```

# *For 循环*

```
*let **arr** = new **Array**(10000000)function **forLoopPerformance**(x){
 let t = Date.now();
 for(let i = 0; i <x.length; i++){i}
 let t2 = Date.now()
 console.log(“for loop: “ , t2-t)
}**forLoopPerformance(arr)
// for loop: 8***
```

# *对于...来说*

```
*let **arr** = new **Array**(10000000)function **forOfPerformance**(x){
 let t = Date.now();
 for(y of x){}
 let t2 = Date.now()
 console.log(“for of: “ , t2-t)
}**forOfPerformance(arr)
// for of: 152***
```

# *因为在*

```
*let **arr** = new **Array**(10000000)function **forInPerformance**(x){
    let t = Date.now();
    for(y in x){y}
    let t2 = Date.now()
    console.log("for in: " , t2-t)
}**forInPerformance(arr)
// for in: 16***
```

# *递归*

```
*let **arr** = new **Array**(10000000)function **recursionPerformance**(x, i, t){
    if(i < x.length){
        i++
        return **recursionPerformance**(x, i, t)

    }
    let t2 = Date.now()
    console.log("for of: " , t2-t)
}**recursionPerformance(arr, 0, Date.now())
// Error Maximum Stack Call***
```