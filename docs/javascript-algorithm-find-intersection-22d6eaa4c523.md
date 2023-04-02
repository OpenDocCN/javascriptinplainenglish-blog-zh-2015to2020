# JavaScript 算法:寻找交集

> 原文：<https://javascript.plainenglish.io/javascript-algorithm-find-intersection-22d6eaa4c523?source=collection_archive---------2----------------------->

![](img/6a58601de25c0856abb81cc430f46546.png)

对于今天的算法，我们将编写一个名为`FindIntersection`的函数，它将两项数组`strArr`作为输入。

我们得到了一个包含两个元素的数组。这两个元素都是字符串，包含以升序排序的逗号分隔的整数。该函数的目标是返回数组中两个字符串中存在的所有整数。该函数应以逗号分隔的字符串形式返回整数。如果数组中的两个字符串不共享任何数字，函数将返回`false`。让我们看一个例子:

```
let strArr = ["1,2,5,6,7", "2,5,7,8,15"];
```

查看我们的数组输入，第一项是`1,2,5,6,7`，第二项是`2,5,7,8,15`。如果我们比较这两个数字，我们可以看到两个字符串中都存在哪些数字。

```
"1,**2**,**5**,6,**7**"
"**2**,**5**,**7**,8,15"
```

该函数将返回`"2,5,7"`，因为这些数字存在于数组的两个字符串中。

现在我们把它变成代码。

我们首先创建两个变量，`arr1`和`arr2`。

```
let arr1 = strArr[0].split(",");
let arr2 = strArr[1].split(",");
```

`arr1`获取数组中的第一个元素，`arr2`获取第二个元素。因为它们是逗号分隔的字符串，所以我们使用`split()`方法，在逗号处分隔字符串，并将它们转换成数组。

我们创建了另一个名为`intersectStrings`的变量，在这个变量中，我们取其中一个数组`arr1`，并使用`filter()`方法只返回两个数组中都存在的数字。

```
let intersectStrings = arr1.filter(function(value){
    return arr2.includes(value);
});
```

您也可以切换顺序，在`arr2`数组上使用 filter 方法，并检查数组中的值是否也存在于`arr1`中。`includes()`方法确定给定的输入是否存在于数组中。

接下来，我们使用 if 语句来检查我们的`intersectStrings`数组是否有任何值。如果为空，这意味着`arr1`和`arr2`不共享任何数字。该函数将返回`false`。

如果数组不为空，那么我们返回数组中的值，但是首先，我们必须以逗号分隔的字符串形式返回输出。

我们使用`join()`方法将数组转换成字符串。我们在方法`","`中包含一个分隔符，用逗号分隔每个字符。我们还使用了`replace()`方法，在正则表达式`/\s/g`的帮助下，我们通过用一个空字符串替换逗号之前的所有空格来删除它们。

```
if(intersectStrings.length === 0){
  return false;
}else{
  return intersectStrings.join(",").replace(/\s/g, "");
}
```

这就结束了我们的功能。以下是剩余的代码:

```
function FindIntersection(strArr) { 
  let arr1 = strArr[0].split(",");
  let arr2 = strArr[1].split(",");

  let intersectStrings = arr1.filter(function(value){
    return arr2.includes(value);
  });

  if(intersectStrings.length === 0){
    return false;
  }else{
    return intersectStrings.join(",").replace(/\s/g, "");
  }
}
```

如果你觉得这个算法有帮助，看看我的其他 JavaScript 算法解决方案:

[](https://medium.com/javascript-in-plain-english/javascript-algorithm-alternating-characters-617eb0764cbc) [## JavaScript 算法:交替字符

### 对于今天的算法，我们将编写一个名为 alternatingCharacters 的函数，它将一个字符串 s 作为…

medium.com](https://medium.com/javascript-in-plain-english/javascript-algorithm-alternating-characters-617eb0764cbc) [](https://medium.com/javascript-in-plain-english/javascript-algorithm-mars-exploration-b6bcc9f3d71e) [## JavaScript 算法:火星探索

### 对于今天的算法，我们将编写一个名为 marsExploration 的函数，它将接受一个字符串 s 作为输入。

medium.com](https://medium.com/javascript-in-plain-english/javascript-algorithm-mars-exploration-b6bcc9f3d71e) [](https://medium.com/javascript-in-plain-english/javascript-algorithm-minimum-distances-65dfde1ca83f) [## JavaScript 算法:最小距离

### 对于今天的算法，我们要写一个名为 minimumDistances 的函数，它将接受一个数组 a 作为…

medium.com](https://medium.com/javascript-in-plain-english/javascript-algorithm-minimum-distances-65dfde1ca83f)