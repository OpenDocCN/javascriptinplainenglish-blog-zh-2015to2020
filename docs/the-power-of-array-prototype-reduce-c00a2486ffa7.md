# Array.prototype.reduce

> 原文：<https://javascript.plainenglish.io/the-power-of-array-prototype-reduce-c00a2486ffa7?source=collection_archive---------9----------------------->

![](img/2cc2d79455ba6984f90ac8a41fe4ce19.png)

Photo by [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 我为什么需要知道？

从技术上来说，你可以忽略这个方法，继续使用心爱的`for`或`while`循环，但是我相信理解这个方法及其背后的方法论是非常值得的。

# 它是什么，它看起来像什么

该方法可用于数组。此方法的目的是将集合缩减为单个值。

```
const result = myArray.reduce((acc, curr, index, array) => {
    ...
}, initialValue);
```

该方法采用两个参数，一个函数和初始值，函数参数如下:

*   `acc` —累计值
*   `curr` —当前元素值
*   `index` —当前元素的索引
*   `array` —调用 reduce 方法的数组

乍一看似乎很多，但是您可以省略不需要的参数。关于如何使用它的最简单的例子是对所有的数组元素求和。

```
const favoriteNumbers = [2, 3, 5, 7, 11, 13, 17, 19, 23, 29];const result = favoriteNumbers.reduce((acc, curr) => {
    return acc + curr; 
}, 0);
```

有必要在每次调用元素后返回新的累积值:`return acc + curr`
如果我们只返回`acc`值，我们将得到`0`作为结果。如果我们只返回`curr`，我们将得到一个等于数组最后一个元素的值，在本例中，如果我们不返回任何东西，我们将得到结果`undefined`。

让我们添加一些控制台日志，以便更好地了解正在发生的事情:

```
const favoriteNumbers = [2, 3, 5, 7, 11, 13, 17, 19, 23, 29];const result = favoriteNumbers.reduce((acc, curr, index) => {
    console.log("\nFunction call on element number: " + index);
    console.log("Current accumulated value: " + acc);
    console.log("Current value: " + curr);
    console.log("New accumulated value: " + (acc + curr));
    return acc + curr;
}, 0);console.log("\nFinal result: " + result);
```

*我确实加了一些* `*\n*` *让输出更好:)*

输出如下所示:

```
Function call on element number: 0
Current accumulated value: 0
Current value: 2
New accumulated value: 2Function call on element number: 1
Current accumulated value: 2
Current value: 3
New accumulated value: 5Function call on element number: 2
Current accumulated value: 5
Current value: 5
New accumulated value: 10Function call on element number: 3
Current accumulated value: 10
Current value: 7
New accumulated value: 17Function call on element number: 4
Current accumulated value: 17
Current value: 11
New accumulated value: 28Function call on element number: 5
Current accumulated value: 28
Current value: 13
New accumulated value: 41Function call on element number: 6
Current accumulated value: 41
Current value: 17
New accumulated value: 58Function call on element number: 7
Current accumulated value: 58
Current value: 19
New accumulated value: 77Function call on element number: 8
Current accumulated value: 77
Current value: 23
New accumulated value: 100Function call on element number: 9
Current accumulated value: 100
Current value: 29
New accumulated value: 129Final result: 129
```

# for 循环呢？

```
let sum = 0;
for(let i = 0; i < favoriteNumbers.length; i++) {
    sum += favoriteNumbers[i];
}
```

这太直接了，不是吗？我为什么要在这里为`reduce`费心呢？

这里是方法论:`for`循环是一种强制性的方法，我们创建了一个变量，然后是一个索引变量来控制访问哪个元素。似乎只是把一堆数字加起来就够了。

`reduce`方法的用法在函数式方法中，我们只需传入一个函数，这个函数将在数组的每个元素上执行。这在一开始可能看起来有点奇怪，因为你必须以不同的方式思考，在我看来这是一件好事，因为它拓宽了如何做事的视角，并为自己决定你更喜欢哪种方法(如果我看到到处都是`for`循环，我会做出刻薄的表情)。

现在是动力部分。

# 真正的力量

我个人不太喜欢这个数字求和的例子，因为它只展示了这个方法最基本的用法——我们多久需要对一组数字求和一次？我们通常在有更多对象的对象中有对象，数字只是分散在两者之间。

我们可以很容易地在任何数组上使用`reduce`方法，而`initialValue`也可以是任何东西，比如一个空白对象，我们可以在其中放置我们感兴趣的数据。

不久前，我想做一个安卓天气应用，只是为了和`React Native`玩玩。我确实找到了一个具有免费层的好 API，但有一个问题，API 以 3 小时的间隔返回带有时间戳的预测，所以我必须转换它们以获得一天中的最低和最高温度。T [这是我使用的 API](https://openweathermap.org/forecast5) ，我也为该应用程序写了一篇文章，可以在这里找到，它有一个包含所有代码的存储库。下面是使用`reduce`的简化片段。

```
data.list.reduce((acc, curr, index) => {
    // Take the first as the current temprature
    if (index === 0) {
        acc.current = {
            currTemp: toCelcius(curr.main.temp)
        }
    } // Group max/min temps by date
    const last = _.last(acc.future);
    if (last) {
        if (getDayOfTheMonth(curr.dt) !== last.date) {
            acc.future.push({
                date: getDayOfTheMonth(curr.dt),
                maxTemp: toCelcius(curr.main.temp),
                minTemp: toCelcius(curr.main.temp),
                recCount: 1,
            });
        } else {
            const currTemp = toCelcius(curr.main.temp);
            if (currTemp > last.maxTemp) {
                last.maxTemp = currTemp;
            } if (currTemp < last.minTemp) {
                last.minTemp = currTemp;
            } last.recCount++;
        }
    } else {
        acc.future.push({
            date: getDayOfTheMonth(curr.dt),
            maxTemp: toCelcius(curr.main.temp),
            minTemp: toCelcius(curr.main.temp),
            recCount: 1,
        });
    }
    return acc;
}, { future: [] });
```

我将`currTemp`直接设置到初始对象上，然后创建一个代表每一天的元素。如果我们的`future`属性中的最后一个元素(代表未来的一天)与当前元素不是同一天，我们创建一个新的条目，否则相应地更新最后一个元素的最低/最高温度。这是一个非常好的数据转换方法。

# 结论

我可以在我的例子中使用一个简单的`for`循环吗？是的，当然！但是我坚信使用`reduce`更干净，这比更少的代码行更重要，这种方法可以让你的数据转换代码看起来像散文！

我们希望编写尽可能少的认知负荷和尽可能少的错误的代码，不直接管理循环甚至使我们更不容易出错。

从技术上来说，我们确实牺牲了一些性能，但这种差别可以忽略不计，尽管有些人可能会认为，编写性能稍差的代码会消耗更多的电力，从而导致全球变暖，但我们不要对此进行讨论。