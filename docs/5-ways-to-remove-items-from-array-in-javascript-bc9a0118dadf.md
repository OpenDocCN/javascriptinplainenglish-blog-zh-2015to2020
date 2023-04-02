# 用 JavaScript 从数组中移除项目的 5 种方法

> 原文：<https://javascript.plainenglish.io/5-ways-to-remove-items-from-array-in-javascript-bc9a0118dadf?source=collection_archive---------3----------------------->

![](img/29e38ebb9ed3867db261ccc102fba581.png)

从数组中移除一个项有多种方法。我们将使用`pop`、`shift`、`splice`、`delete`和`length`从数组中移除物品。让我们简单地逐一讨论这 5 种方法。

# pop()方法

此方法从数组末尾移除一个元素。它返回被删除的值。

```
const countries = ['India', 'US', 'Canada', 'France']; const removedItem = countries.pop(); console.log(countries); // ['India', 'US', 'Canada']console.log(removedItem); // France
```

# shift()方法

此方法从数组的开头移除一个元素，并返回移除的元素。

```
const phones = ['Nokia', 'Samsung', 'Apple']; const removedItem = phones.shift(); console.log(phones); // ['Samsung', 'Apple']console.log(removedItem); // Nokia
```

# splice()方法

此方法可以在数组的指定索引处移除和添加项。

*   `splice()`的第一个参数取一个数组索引，在这里你要添加或删除项目。
*   第二个参数接受要从指定索引中移除的元素数量。如果不删除任何项目，则该值可以为 0。
*   第三个参数接受要在指定索引处添加的项。如果我们只是删除，那么这可以留空。我们可以添加任意多的值。

```
const language = ['JavaScript', 'Java', 'SQL', '.NET']; language.splice(2, 1); console.log(language); //['JavaScript', 'Java', '.NET']
```

我们也可以同时删除和添加新元素。

```
const language = ['JavaScript', 'Java', 'SQL', '.NET'];language.splice(2, 2, 'Android', 'Swift'); console.log(language); 
//['JavaScript', 'Java', 'Android', 'Swift']
```

splice 方法返回已删除项的数组。

```
const numbers = [20, 40, 60, 80]; console.log(numbers.splice(1, 2)); // [40, 60]
```

# delete 关键字

`delete`关键字用于删除一个对象的属性。这可以用来从数组中删除任何元素。关键字`delete`删除元素，但是在那个位置留下一个未定义的值。

```
const games = ['Cricket', 'Football', 'Hockey']; delete games[2]; console.log(games); 
// ['Cricket', 'Football', undefined]
```

# 使用数组长度

如果我们想从一个数组的末尾删除一些指定数量的元素，那么我们只需将数组的`length`属性设置为数组的原始长度减去要删除的元素数量。

```
const numbers = [10, 20, 30, 40, 50]; numbers.length = 3; // to remove two elements from endconsole.log(numbers); // [10, 20, 30]
```

**同时检查** : [6 种向数组添加项目的方式](https://jscurious.com/how-to-add-items-to-an-array-in-javascript/)

*感谢您的宝贵时间* ☺️
更多网络开发博客，请访问[jscurious.com](http://jscurious.com/)