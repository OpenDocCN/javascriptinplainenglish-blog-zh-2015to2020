# JavaScript 算法:最小距离

> 原文：<https://javascript.plainenglish.io/javascript-algorithm-minimum-distances-65dfde1ca83f?source=collection_archive---------2----------------------->

![](img/7c53a4ae76c6d0a39e714df3c55362fa.png)

对于今天的算法，我们将编写一个名为`minimumDistances`的函数，它将接受一个数组`a`作为输入。

在这个函数中，我们得到了一个整数数组。每个值都是通过其索引找到的。这个函数的目标是找到数组中任何一对匹配值，并通过计算它们的索引差来确定这两个匹配值之间的距离。计算差异后，确定哪个匹配对具有最小的距离并输出该值。如果不存在匹配对，该函数将返回-1。这里有一个例子:

```
let a = [2,5,3,7,2,3];
```

在上面的示例数组中，唯一具有匹配值的数字是 **2** 和 **3** 。

2 的索引是 a[0]和 a[4]。如果我们减去指数`4 — 0 = 4`，我们得到 4。

3 的索引是 a[2]和 a[5]。如果我们减去指数`5 — 2 = 3`，我们得到 3。

在两个距离(4 和 3)中，该函数将返回最小距离值 3。

让我们把这个转换成代码。

```
let distances = [];
```

`distances`变量将是保存所有匹配对与我们的输入数组的距离的数组。

```
for(let i = 0; i<a.length; i++){
    if(a.indexOf(a[i]) !== a.lastIndexOf(a[i])){
      let minDistance = a.lastIndexOf(a[i]) - a.indexOf(a[i]);
      distances.push(minDistance);
    }
}
```

我们在输入和数组中循环，首先检查迭代的数字在输入数组中是否匹配。我们使用`indexOf()`，它返回第一个出现的指定值的索引。我们还使用了`lastIndexOf()`，它返回最后出现的指定值的索引。如果两种方法返回相同的索引，那么这意味着没有匹配对。如果两种方法返回不同的索引，那么这意味着数组中有另一个匹配的数字。

```
let minDistance = a.lastIndexOf(a[i]) - a.indexOf(a[i]);
distances.push(minDistance);
```

当这种情况发生时，我们取匹配数字的索引并减去它们。我们将差异或距离放入我们的`distances`数组。

```
if(distances.length === 0){
    return -1;
}else{
    distances.sort(function(a, b) {
      return a - b;
    });

    return distances[0];
}
```

如果一个数组没有匹配对，我们会知道，因为我们的`distances`数组将保持为空。如果这是真的，那么我们将返回-1。

如果没有，那么我们将返回最小的距离。我们通过对数组排序来做到这一点。我们使用`sort()`方法和`sort()`方法中的比较函数对数组进行排序。比较函数通过将我们的值作为数字而不是字符串进行比较来帮助定义排序顺序(默认方法)。这是为了防止当我们的数组包含一位数和多位数时出现奇怪的排序顺序。

既然我们的数组是从最小到最大排序的，我们只关心最小的距离，所以我们返回`distances`数组中的第一个值。

下面是代码的其余部分:

```
function minimumDistances(a) { let distances = []; for(let i = 0; i<a.length; i++){
        if(a.indexOf(a[i]) !== a.lastIndexOf(a[i])){ let minDistance = a.lastIndexOf(a[i]) - a.indexOf(a[i]);
            distances.push(minDistance);
        }
    }

    if(distances.length === 0){
        return -1;
    }else{
        distances.sort(function(a, b) {
          return a - b;
        });

        return distances[0];
    }
}
```

如果您认为这个算法有用，请查看我的其他 JavaScript 算法解决方案文章:

[](https://medium.com/@endubueze00/javascript-algorithm-viral-advertising-168a872cb557) [## JavaScript 算法:病毒式广告

### 对于今天的算法，我们将编写一个名为 viralAdvertising 的函数，它将接受一个整数 n 作为…

medium.com](https://medium.com/@endubueze00/javascript-algorithm-viral-advertising-168a872cb557) [](https://medium.com/javascript-in-plain-english/javascript-algorithm-sock-merchant-de9ffa754dfc) [## JavaScript 算法:袜子商人

### 对于今天的算法，我们将编写一个名为 sockMerchantthat 的函数，它将接受两个输入，一个整数 n…

medium.com](https://medium.com/javascript-in-plain-english/javascript-algorithm-sock-merchant-de9ffa754dfc)