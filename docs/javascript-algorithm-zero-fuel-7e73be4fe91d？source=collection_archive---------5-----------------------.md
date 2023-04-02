# JavaScript 算法:零燃料

> 原文：<https://javascript.plainenglish.io/javascript-algorithm-zero-fuel-7e73be4fe91d?source=collection_archive---------5----------------------->

## 我们写了一个函数，它将决定你减少的燃料量是否足够让你到达最近的加油站

![](img/3a74ca56d31d85e579691105543b44f1.png)

我们将编写一个名为`zeroFuel`的函数，它将接受三个整数(`distanceToPump`、`mpg`和`fuelLeft`)作为参数。

你和你的朋友在内华达州中部露营。太阳升起来了，该回家了。你坐进你的车，发现它的汽油量不太理想。你知道在沙漠里，尤其是内华达州，没有多少公共服务设施，比如高速公路旁的加油站。

你看到你的车还有`fuelLeft`加仑的剩余汽油，你的车每加仑跑`mpg`英里。最近的加油站在`distanceToPump`英里外。

该函数的目标是确定是否有可能到达加油站。如果可能，返回`true`，如果不可能，返回`false`。

这里有一个例子:

```
let distanceToPump = 50;
let mpg = 25;
let fuelLeft = 2;
```

最近的加油站在几英里之外。你的车还有`2`加仑的汽油，每加仑跑`25`英里。在这种情况下，函数将返回`true`。如果汽车每加仑行驶 25 英里，而你还有 2 加仑剩余，这意味着你还能行驶`25 * 2 = 50`英里。虽然你的车会在加油时停下来，但你会成功的。

如果`distanceToPump`是 100，该函数将返回 false，因为您的汽车在汽油耗尽之前只能行驶一半。

让我们从创建一个名为`distanceLeft`的变量开始。这个变量将保持你的汽车在耗尽汽油之前可以行驶的最大距离。我们通过将`fuelLeft`乘以`mpg`得到这个数字。

```
let distanceLeft = mpg * fuelLeft;
```

如果您的汽车在剩余汽油的情况下可以行驶的距离大于或等于到泵的距离，该功能将返回`true`。否则，下面的条件将返回`false`。

```
return distanceLeft >= distanceToPump;
```

下面是该函数的其余部分:

```
function zeroFuel(distanceToPump, mpg, fuelLeft){
  let distanceLeft = mpg * fuelLeft; 
  return distanceLeft >= distanceToPump;
}
```

如果你觉得这个算法有帮助，看看我的其他 JavaScript 算法解决方案:

[](https://medium.com/@endubueze00/javascript-algorithm-return-largest-numbers-in-arrays-476914b717a7) [## JavaScript 算法:返回数组中最大的数字

### 我们编写了一个函数，从数组参数中返回一个数组，该数组包含每个子数组中最大的数字

medium.com](https://medium.com/@endubueze00/javascript-algorithm-return-largest-numbers-in-arrays-476914b717a7) [](https://levelup.gitconnected.com/javascript-algorithm-remove-the-final-exclamation-mark-8a7c3c8cd3a2) [## JavaScript 算法:去掉最后一个感叹号

### 我们要写一个函数来删除字符串末尾的感叹号

levelup.gitconnected.com](https://levelup.gitconnected.com/javascript-algorithm-remove-the-final-exclamation-mark-8a7c3c8cd3a2) [](https://medium.com/javascript-in-plain-english/javascript-algorithm-remove-first-and-last-character-a80b7bc28536) [## JavaScript 算法:删除第一个和最后一个字符

### 我们编写了一个函数来删除字符串的第一个和最后一个字符

medium.com](https://medium.com/javascript-in-plain-english/javascript-algorithm-remove-first-and-last-character-a80b7bc28536) 

## **用简单的英语写的 JavaScript 的注释:**

我们总是有兴趣帮助推广高质量的内容。如果你有一篇文章想用简单的英语提交给 JavaScript，用你的中级用户名发邮件到 submissions@javascriptinplainenglish.com[给我们，我们会把你添加为作者。](mailto:submissions@javascriptinplainenglish.com)