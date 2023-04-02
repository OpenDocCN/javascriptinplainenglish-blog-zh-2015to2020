# 变异 JavaScript 数组

> 原文：<https://javascript.plainenglish.io/mutating-javascript-arrays-377722878ffa?source=collection_archive---------4----------------------->

## 在 JavaScript 中改变数组的能力是 JS 工具箱中必不可少的。我们来看几个方法。

![](img/da7adc3463e119e80ccd35f65df244ab.png)

Photo by [Artem Sapegin](https://unsplash.com/@sapegin?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 推送()

让我们看看下面的数组。

```
var numbers = [0, 3, 6, 9, 12];
```

`numbers`是包含递增 3 的数字的数组。如果我们想在序列的最后加上下一个数字呢？我们可以使用 push()方法来做到这一点。

```
numbers.push(15);// result - numbers = [0, 3, 6, 9, 12, 15]
```

当数组的最终大小未知并且需要添加一个值时，push()是一个非常有用的工具。

# 流行()

与 push()相反，pop()移除数组末尾的值。让我们来看看。我们将使用相同的`numbers`数组。

```
// numbers = [0, 3, 6, 9, 12, 15]numbers.pop();// result - numbers = [0, 3, 6, 9, 12]
```

当需要删除数组的最后一个值时，pop()非常有用。更好的是，当 pop()被调用时，不仅删除数组的最后一个值，而且*还返回*那个值。

```
// numbers = [0, 3, 6, 9, 12]var lastVal = numbers.pop();// result - lastVal = 12; numbers = [0, 3, 6, 9]
```

注意`lastVal`是 12，但是`numbers`也从数组的末尾移除了 12。在这种情况下，如果变量需要数组的最后一个值，但 numbers 数组不再需要它，那么 pop()就很有用。

如果变量需要数组的最后一个值，但*不需要删除*，则使用这个。

```
// numbers = [0, 3, 6, 9]// store last value but do NOT remove the value from numbers
var val = numbers[numbers.length - 1];// result - val = 9; numbers = [0, 3, 6, 9]
```

`val`是索引`numbers.length — 1`处的值。`numbers.length = 4`但是最后一个指标是 3。所以我们需要把`numbers.length`减一。在这种情况下，`numbers[numbers.length — 1]`与`numbers[3]`相同。

# shift()

shift()移除并返回数组的第一个元素。我们再用`numbers`吧。

```
// numbers = [0, 3, 6, 9]var shiftVal = numbers.shift();// result - shiftVal = 0; numbers = [3, 6, 9]
```

最后，我们来看看如何在数组的开头添加一个值。

# 未移位()

unshift()类似于 push()。主要区别在于它将值添加到数组的开头。让我们将 0 加回数组中。

```
// numbers = [3, 6, 9]numbers.unshift(0);// result - numbers = [0, 3, 6, 9]
```

unshift()的巧妙之处在于可以一次添加多个值。

```
// numbers = [0, 3, 6, 9]numbers.unshift(1,2,3);// result - numbers = [1, 2, 3, 0, 6, 9]
```

# 结论

JavaScript 为数组提供了更多的方法。但是，这是一个好的开始。push()、pop()、shift()和 unshift()现在已经添加到您的 JavaScript 工具箱中了！去征服阵列吧！