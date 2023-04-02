# JavaScript 中的 map()函数会使数组变异吗？

> 原文：<https://javascript.plainenglish.io/does-map-mutate-array-c21843c2d2fe?source=collection_archive---------9----------------------->

![](img/8b89299d391f578a7292bb72cc4ba46f.png)

Photo by [Ross Findon](https://unsplash.com/@rossf?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

map 用于迭代元素数组。它运行在所有数组元素上提供的回调，并返回一个新数组。这里有一个例子:

```
const arr = [1,2,3,4,5]const doubleArr = arr.map(a => a*2);console.log(arr); //**[1,2,3,4,5]**console.log(doubleArr); //**[1,4,6,8,10]**
```

到目前为止一切顺利。我们运行了数组上提供的回调，得到了一个新的回调，没有改变原来的数组。

但是如果我们有一个对象数组会发生什么呢？我们去看看。

```
const fruits = [ { id: 1, name: "orange", inStock: true }, { id: 2, name: "mango", inStock: false }, { id: 3, name: "kiwi", inStock: true }];const fruitsUpdated = fruits.map(fruit => { if (fruit.name === "mango") { fruit.inStock = true; } return fruit;});
```

因此，我们上面所做的是检查水果名称是否为 **mango** ，并将其 **inStock** 属性更改为 true。现在让我们尝试输出数组:

```
console.log(fruits);
[ { id: 1, name: 'orange', inStock: true },
  { id: 2, name: 'mango', **inStock: true** },
  { id: 3, name: 'kiwi', inStock: true } ]console.log(fruitsUpdated);
[ { id: 1, name: 'orange', inStock: true },
  { id: 2, name: 'mango', **inStock: true** },
  { id: 3, name: 'kiwi', inStock: true } ]
```

我们的**结果更新的**数组似乎被正确更新了，但是我们也注意到我们的原始数组也被修改了。

这是因为 javascript 不是在水果数组的每个索引处存储实际的对象，而是存储一个指针，该指针引用对象在内存中的存储位置。所以。map()正在创建一个新数组，但它正在用指向相同对象的指针填充该数组。

我们可以通过为每次迭代在地图中创建一个新对象来解决这个问题。这是我们新的水果更新功能:

```
const fruitsUpdated = fruits.map(fruit => { const fruitCopy = { ...fruit }; if (fruitCopy.name === "mango") { fruitCopy.inStock = true; }
    return fruitCopy;});
```

我们在回调中为 fruit 对象创建了一个新的引用，这样如果我们对它进行修改，它就不会更新原来的 fruits 数组。

这是新的日志:

```
console.log(fruits);
[ { id: 1, name: 'orange', inStock: true },
 { id: 2, name: 'mango', **inStock: false** },
 { id: 3, name: 'kiwi', inStock: true } ]console.log(fruitsUpdated);
[ { id: 1, name: 'orange', inStock: true },
 { id: 2, name: 'mango', **inStock: true** },
 { id: 3, name: 'kiwi', inStock: true } ]
```

您可以看到原始数组保持不变。

# 结论

使用时要记住的事项。地图():

*   仔细编写回调函数，因为它可以修改原始数组。
*   在回调函数中，总是为原始数组中的每个对象创建新对象。否则，您将只是复制指向原始对象的指针。
*   如果数组中有深度嵌套的对象，请确保正确地克隆了它，这样原始数组就不会变异。