# JavaScript 算法:生日蛋糕蜡烛

> 原文：<https://javascript.plainenglish.io/javascript-algorithm-birthday-cake-candles-86da2c686634?source=collection_archive---------6----------------------->

![](img/8d35f7a492cd4e7e579b48b1fb5cbe14.png)

对于今天的算法，我们将创建一个名为生日蛋糕蜡烛的函数。

为了解释这个功能，我们假设你负责你侄女生日的蛋糕。你是她的叔叔，你真的不想参加她的生日聚会，因为那意味着你要在一个充满尖叫的四至六岁儿童的房间里度过几个小时，而不是在家里和你的兄弟们玩电子游戏。

![](img/477412bc4f0d0254646e20d6daf18450.png)

但是作为一个好叔叔，你得到了蛋糕。你决定蛋糕在她整个年龄的每一年都有一支蜡烛。当她吹灭蜡烛时，她只能吹灭最高的蜡烛。

![](img/5e766f8c9c840b0ca98383ea8888ec38.png)

因此，如果你的侄女即将满 4 岁，这将意味着我们的生日蛋糕蜡烛函数将有一个四个数字的数组。假设蜡烛的高度如下:

```
[2,4,4,1]
```

因为她只能吹最高的蜡烛，所以在上面的数组中 4 是最高的数字。有两根高度为 4 的蜡烛，因此该函数将输出 2。你的侄女会吹灭那两支蜡烛。

为了将它转换成代码，我们将使用 Math.max 方法找出数组中最大的数字。让我们将这个数字赋给变量 **maxNum** :

```
let maxNum = Math.max(...ar); // ar is the array variable
```

接下来，我们将使用过滤方法。如果您想更详细地了解 filter 方法是如何工作的，您可以阅读这篇文章，其中我解释了 filter 方法在 Mini-Max Sum 算法中是如何工作的:

[](https://medium.com/@endubueze00/mini-max-sum-a62de1d7f4f1) [## 最小-最大和

### 对于今天的算法，我们将创建一个名为 miniMaxSum 的函数。在这个函数中，给你一个数组…

medium.com](https://medium.com/@endubueze00/mini-max-sum-a62de1d7f4f1) 

简而言之，filter 方法过滤掉数组中不符合 return 语句中特定条件的所有值。

```
let filtered = ar.filter(function(value, index, arr) {
    return value === maxNum;
});
```

在这种情况下，filter 方法将只返回等于 **ar** 数组中最大数字的值。所有不等于最高数字的值都不会进入。filter 方法创建了一个接受值的新数组，因此我们将它赋给被过滤的变量。

现在我们有了一个只有最大值的数组，我们使用 Array.length 属性计算数组的长度，告诉我们你的侄女可以吹多少根蜡烛。我们返回那个号码。

```
return filtered.length;
```

以下是完整的代码:

```
function birthdayCakeCandles(ar) {
    let maxNum = Math.max(...ar);
    let filtered = ar.filter(function(value, index, arr) {
        return value === maxNum;
    }); return filtered.length;
}
```

这就是我们的算法。

![](img/31426913b0187d0df007374b621b2281.png)