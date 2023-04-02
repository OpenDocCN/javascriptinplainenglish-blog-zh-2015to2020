# 每个开发人员都应该知道的 JavaScript 数组方法

> 原文：<https://javascript.plainenglish.io/javascript-array-methods-every-developer-should-know-about-a43e3783627e?source=collection_archive---------7----------------------->

## 学习这些数组方法来利用您的 JavaScript 编码技能

![](img/23145c72d3d8f913ff4209d6690e6dcf.png)

Photo by [Caspar Camille Rubin](https://unsplash.com/@casparrubin?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 在 JavaScript 中，数组是一个特殊的变量，用于在单个元素中存储各种值。

它包含不同类型的数组方法，我们可以添加、删除、修改和使用它们，以我们想要的方式在代码中工作。了解它们可以提高你作为专业开发人员的能力。

## 把数组想象成一个包含各种其他对象的对象。

例如:如果你创建了一个数组" *room"* ，它可以包含像*床、椅子、桌子、风扇、镜子等*的元素。如果把“天空”做成一个数组，*星星、月亮、太阳、行星等。*可为其元素。如果把一个“城市”做成一个数组，*街道，郊区，公园，房屋，*都可以是它的元素。如果你把年龄作为一个数组，任何数字都可以是它的元素。

## 它表示为:

房间= [床，椅子，桌子，风扇，镜子]；

天空= [星星，月亮，太阳，行星]；

城市= [街道，郊区，公园，房子]

年龄= [ 20，22，25，30，23，32]

这真的取决于你的想象力。您可能已经注意到，可以把它想象成包含许多条目的超集，它包含在大括号[]中。

这只是一个非常基本的知识。让我们跳到了解更多关于数组的有趣部分。

# 一些()

此方法使用作为参数传递的函数测试数组。然后，如果找到其中一个值，它将返回`true`，如果没有找到，它将返回`false`。

```
const cameraPrice = [ 1000, 2000, 1500, 2500, 3000 ]cameraPrice.some(camera => camera >= 1500)//returns true
```

# 地图()

此方法将一个函数作为参数，该参数返回传递的数组中每个元素的图像。它总是返回与传递的元素数量相等的元素。

```
const num = [4, 9, 16, 25, 36, 49, 64 ]num.map(Math.sqrt)// returns 2, 3, 4, 5, 6, 7, 8
```

# 减少()

它将传递的数组中的多个元素缩减为一个值。从左到右对数组的每个值执行此方法。返回值放在一个元素中，也称为累加器(total，result)

```
const sum = [ 2, 4, 6, 8 ]sum.reduce( (total, value) => total + value )// returns 2 + 4 + 6 + 8 = 20
```

# 过滤器()

该方法接收一个函数作为参数，并返回数组中返回的元素`true`。

```
const cars = [ { id = 1, name = “Mazda” },{ id = 2, name = “Audi” }, {id = 3, name = “Tesla”}, {id = 4, name = “Ford”} ]cars.filter( car => car.name === “Tesla” )//returns Tesla
```

# forEach()

顾名思义，它为每个元素调用函数。为了运行此方法，数组应该有值。

```
const cars = [{ id = 1, name = “Mazda” },{ id = 2, name = “Audi” },{id = 3, name = “Tesla”},{id = 4, name = “Ford”}]cars.forEach( car => console.log(car.name) )//returns 
// Mazda
// Audi
// Tesla
// Ford
```

# 查找()

该方法将一个函数作为数组，该数组返回通过测试的第一个元素的值。在找到第一个值后，它不会检查其他值。如果没有找到该值，它将返回`undefined`。

```
const cars = [{ id = 1, name = “Mazda” },{ id = 2, name = “Audi” },{id = 3, name = “Tesla”},{id = 4, name = “Ford”}]cars.find( car => car.id === 4 )
cars.find( car => car.id === 6 )//returns Ford
//returns undefined
```

# 排序()

此方法将函数作为参数，并以排序的形式返回数组的元素。排序可以是`alphabetic`或`numeric`和`ascending`或`descending`

```
const num = [ 2, 9, 3, 5, 1, 8, 7, 6, 4 ]num.sort( (a, b) => a-b )num.sort( (a, b) => b-a )//returns 1, 2, 3, 4, 5, 6, 7, 8, 9//returns 9, 8, 7, 6, 5, 4, 3, 2, 1
```

# concat()

这个数组方法将多个数组连接成一个数组。

```
const num = [ 1, 2, 3, 4 ]cosnt nextNum = [ 5, 6, 7, 8 ]num.concat(nextNum)//returns [1, 2, 3, 4, 5, 6 ,7, 8]
```

# 反向()

此方法反转数组中的元素。第一个元素成为最后一个，最后一个成为第一个。

```
const num = [ 1, 2, 3, 4 ]num.reverse()//returns [4, 3, 2, 1]
```

# 包括()

如果数组包含某个被传递的元素，该方法返回`true`，如果没有找到，则返回`false`。

```
const num = [ 1, 2, 3, 4 ]num.includes(4)
num.includes(5)//returns true
//returns false
```

数组简单而庞大。它提高了你作为开发人员的编码技能，并有助于提高你的学习速度。

试着在你的浏览器上运行它，并不断练习。这些是每个开发人员都应该知道的 JavaScript 数组方法。

# 喜欢你读的吗？

如果你想阅读更多像这篇文章这样的信息丰富的文章，我的电子邮件摘要正是你想要的。

[*点击加入我今天的邮件摘要。*](https://upscri.be/zm7qsy)