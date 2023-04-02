# 是否应该停止使用对象和数组来存储数据？

> 原文：<https://javascript.plainenglish.io/stop-using-objects-and-arrays-to-store-data-289c3edaaa33?source=collection_archive---------0----------------------->

## ES6 以集合和映射的形式有其他处理数据结构和值的方法

![](img/039eb101b34710e6dbc8eb39033c1091.png)

Photo by [Alvaro Reyes](https://unsplash.com/@alvarordesign?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

多年来，程序员一直使用对象和数组来存储数据。这种趋势不仅仅局限于 JavaScript。

除了这两种存储多值和处理数据结构的方法之外，根本没有其他选择。

然而，在使用对象和数组时有一些限制，例如:

1.  数组可以存储重复的元素。
2.  没有方法可以像数组一样找到对象的长度。
3.  只有字符串可以存储在对象中，它不记得插入顺序。
4.  开发人员必须根据他们的用例选择数组或对象。
5.  像 [Lodash](https://lodash.com/docs/) 这样的第三方库被用来增强阵列的功能

但是随着 2015 年 ES6 的推出，事情变得更好了。

ES6 引入了对 Map 和 Set 的支持，旨在克服上述限制。

## 什么是集合和映射？

如前所述，这两者都是在 JavaScript 的 ES6 版本中引入的。

集合是唯一元素的有序集合。“唯一元素”是主要的要点，因为它意味着集合中不能存储重复的元素。但是它没有键值对系统。

另一方面，映射是数组和对象数据结构的组合。它是一个类似对象的键-值对的集合，但是它还记得插入格式，并且有一个 length( `.size`)属性。

我会先看一遍布景，然后看地图。

## 集合的声明和初始化

集合可以这样初始化:

```
const set = new Set();
```

## 从集合中添加和移除元素

您可以使用`.add()`方法轻松地将元素插入集合中。

```
const set = new Set();set.add('John');set.add('Martha')set.add('Bryan');set.add('John');// set = {'John','Martha','Bryan'}
```

这就是事情变得有点有趣的地方。JavaScript 中的集合借用了数学集合的许多属性，并且只包含唯一的元素。

删除元素也非常简单，使用`.delete()`方法删除单个元素，使用`.clear()`方法删除所有元素

```
set.add('John');set.add('Martha')set.add('Bryan');set.delete('Martha')//set = {'John','Bryan'}set.clear(); // removes all the element
```

## 集合的大小

使用`.size`方法，人们可以很容易地找到有帮助的集合的大小。

```
set.add('a')set.add('b');set.add('c');console.log(set.size) // => 3
```

## 访问集合中的元素

Set 在尝试记录或访问其值时表现不同。您可以记录数组并查看元素，但对于集合来说就不一样了。

```
var arr=[1,2,3];const set = new Set(arr);console.log(set) // => [object Set]console.log(arr) // => (3) [1,2,3]
```

为了访问 Set，我们需要一个`SetIterator()`来获取所有的值。

JavaScript 提供了一个属性`.values()`来获取一个迭代器，然后我们可以结合一个循环来检索所有的值。

下面的代码片段演示了这一点:

```
var arr=[1,2,3];const set = new Set(arr);var iterator=set.values()console.log(iterator.next().value) //1
```

检索所有元素的更简单方法是使用`.forEach()`，如下所示:

```
var arr=[1,2,3];const set = new Set(arr);set.forEach(*v*=>console.log(v))
```

输出:

```
1
2
3
```

此外，您可以使用`.has()`方法检查一个值是否存在，如果找到该元素，该方法将返回 true。

```
var arr=[1,2,3];const set = new Set(arr);console.log(set.has(1)); // true
```

值得一提的是，像`keys()` 和`entries()`这样的方法对于 Set 是可用的，尽管 Set 不支持键值对元素。

## 集合与数组

集合和数组倾向于执行和处理相同的操作，但是存在一些差异。

最大的区别是 Set 不能有像 Array 一样的重复项，Set 提供了一个更简单的方法来删除项。

此外，集合中的元素在插入顺序上是可迭代的。

与数学集合一样，JavaScript 中的集合也可用于执行并集和交集等操作，这些操作可在合并数据或查找两个集合中的公共元素时使用。

您可以在此查看设置[的方法和指南的完整列表。](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set)

## 初始化和声明映射

与 Set 类似，Map 也可以用同样的方式声明。

```
const map = new Map();
```

## 从地图中添加和移除元素

该映射支持类似对象的键值对。因此，在增加价值时，我们也需要提供一个密钥。

这与我们在电视上看到的不同。

```
const map = new Map();map.set('Name', 'iPhone'); // map.set(key,value) formatmap.set('Brand', 'Apple');map.set('Price', '$1000');
```

要从映射中删除一个值，我们可以简单地将键传递给`.delete()` 属性。

```
const map = new Map();map.set('Name', 'iPhone'); map.set('Brand', 'Apple');map.set('Price', '$1000');map.delete('Price'); //removes the element with key 'Price'
```

像 Set 一样，我们可以使用`.clear()`来移除所有的元素。

```
map.clear() // removes all the element
```

## 地图大小

使用`.size`属性可以很容易地检索地图的大小(长度)。

```
const map = new Map();map.set('Name', 'iPhone');map.set('Brand', 'Apple');map.set('Price', '$1000');console.log(map.size)//=> 3
```

## 访问地图中的元素

Map 为我们提供了一个. get()方法，通过在方法中将键作为参数传递来快速获取值。

```
const map = new Map();map.set('Name', 'iPhone');map.set('Brand', 'Apple');map.set('Price', '$1000');console.log(map.get('Name')); //iPhoneconsole.log(map.get('Brand')); // Apple
```

但是如果您只想要键、值或者键和值都想要呢？

Map 分别有`.keys()`、`.values()`和`.entries()`用于实现相同的功能。

使用上面代码中的同一张地图，

```
console.log(map.keys());
// iterator {'Name','Brand',Price'}console.log(map.values());
// iterator {'iPhone','Apple','$1000'}console.log(map.entries());
//iterator {'Name':'iPhone','Brand':'Apple',Price':'$1000'}
```

迭代地图也很简单。

```
//with for-each
map.forEach((value, key) => {
   console.log(`${key} is ${value} years old!`);
});

// with for-of
for(const [key, value] of map) {
  console.log(`${key} : ${value}`);
}
```

此外，使用`.has()`属性并传递键，可以很容易地检查元素是否存在。

```
var map = new Map();
map.set('age',19);console.log(map.has('age')) // true since 'age' key is present
```

## 将对象转换为地图

如果您决定将对象转换成地图，JavaScript 会帮您解决。

我们之前已经使用了`.entries()`来获取所有的键值对，但是这次我们将使用`Object`上的方法。

```
const myObject= {
  'Age': '25',
  'Gender': 'Male',
  'Nationality': 'Australian'
};

const myMap = new Map(Object.entries(myObject)); //object to mapconst anotherObject = Object.fromEntries(myMap) // map to object
```

你可以很容易地将一张地图转换成一个物体，如上所示。

要将地图转换成数组，我们可以使用`Array.from(myMap)`。

## 贴图与数组和对象

这个图似乎解决了数组和对象的很多缺点，比如能够处理复杂得多的操作。

地图就像是数组和对象的混合体。它有一个类似 array 的 size 属性，可以用键值对格式存储元素。

除此之外，它还提供了类似于`.has()`的方法来检查一个元素是否存在，这可以节省您大量的时间。

此外，它不一定需要密钥是字符串类型。你甚至可以使用一个对象作为一个键来帮助你写更好的代码。

你可以在这里找到更详细的指南[。](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)

## 最后的想法

虽然数组和对象已经成为存储集合和键值对元素的事实上的标准，但是随着 Map 和 Set 的引入，您可以为您的代码提供一种有趣的方法。

Set 和 Map 是 JavaScript 为存储复杂数据结构提供的新标准。

此外，使用这些数据结构还消除了使用像 [Lodash](https://lodash.com/) 这样的第三方库的需要，因为这些新的数据结构默认提供了像`.has()`和`.delete()`这样的方法。

数组和对象在任何意义上都没有过时，但是，使用集合和映射肯定是处理数据的更好的方法，尤其是在构建巨大、复杂的应用程序时。

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**