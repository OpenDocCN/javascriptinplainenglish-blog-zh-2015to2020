# 什么是 JavaScript 数组？

> 原文：<https://javascript.plainenglish.io/what-are-javascript-arrays-e7819fb5e44a?source=collection_archive---------6----------------------->

在 JavaScript 中，你可能经常会遇到的极其重要和有用的东西是数组。数组是 web 开发的基本构件之一，你越早完全理解它们，你就会越好，因为它们被广泛使用。今天，我们将了解数组及其用途，以及如何自己开始使用它们。

![](img/60089be390b7c1a94b6effe65e8940df.png)

# 什么是数组？

描述数组最基本的方式就是简单地描述一个可以同时有多个值的变量。假设你要描述你每天开着什么样的车辆去上班。通常，我们必须根据不同的车辆来设置代码:

如您所见，控制台将打印“您好，我是 Alex。我开喷气式飞机上班！”然而，如果我们需要的不是 5 辆车的列表，而是 500 辆车的列表呢？理论上，您可以写出所有 500 辆车的列表，并以同样的方式完成，但这里更好的方法是使用数组。让我们看看如何用一个数组而不是一个变量列表来写一些非常相似的代码。

如您所见，这段代码非常简洁，它允许我们写出数组中的每辆车，而不是为每辆车声明一个新变量。这将为您的代码编辑器节省大量的时间和空间！

![](img/4722ef68cd12e3e457158bf8f032b758.png)

# 如何设置阵列

制作一个数组很容易，几乎和设置一个变量一样。我们像一个变量(var、let 或 const)一样开始数组，我们用 [camel case](https://en.wikipedia.org/wiki/Camel_case##targetText=Camel%20case%20(stylized%20as%20camelCase,no%20intervening%20spaces%20or%20punctuation.) 给数组命名，然后我们将它设置为一组方括号，后跟一个分号。数组的各个部分也称为元素，放在方括号内，每个部分用逗号分隔。这些元素可以是字符串、数字、布尔以及变量可以是的任何东西。让我们再看一下我们之前制作的阵列:

这个数组包含五个不同的元素，它们都是字符串，我们可以通过声明数组的名称以及我们希望从该数组中得到的元素(通过索引号)来调用它们，如 console.log 语句中所示。然而，在处理数组时，还有一件事需要注意。如果你认为“我的朋友史莱克开着 ***公交车*** 去上班！”就是将要在控制台上显示的内容，那么您实际上是错误的。实际将要显示到控制台的是“我的朋友史莱克开着一辆 ***摩托车*** 去上班！”。但这是为什么呢？显然，bus 是数组中的第三个元素，我们在 console.log 语句中通过键入 arrayOfVehicles[ **3** ]调用了数组中的第三个元素，对吗？事实并非如此，因为数组不是从 1 开始计数元素，而是从 0 开始计数元素。这些被称为数组的索引号，这就是为什么显示摩托车而不是公共汽车。公交车算 2 而不是 3，摩托车算 3 而不是 4 等等。

![](img/4722ef68cd12e3e457158bf8f032b758.png)

# 数组和对象有什么区别？

从技术上讲，数组是一个对象，但是，它是一种特殊类型的对象。对象和数组的主要区别在于数组元素是用一个索引号来调用的，而对象中的东西是用它的名字来调用的。看看下面的代码，自己体会一下不同之处。

如您所见，数组使用索引号 2 将 kiwi 打印到控制台，对象使用名称 fruit3 将 kiwi 打印到控制台。

![](img/4722ef68cd12e3e457158bf8f032b758.png)

# 附加说明

在使用数组时，另一件非常有用的事情是一些属性和方法，您可以使用它们来做各种各样的事情。例如，如果我们有一个名为 toppings 的数组，其中有 10 种不同的 toppings 作为每个元素，我们可以使用。length 属性，通过键入 toppings.length 查看数组中有多少元素。有关数组的所有属性和方法的完整列表，请查看下面的链接。

[](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array#) [## 排列

### JavaScript 数组类是一个全局对象，用于构造数组；哪些是高层次的…

developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array#) 

*来源:*

[https://www.w3schools.com/js/js_arrays.asp](https://www.w3schools.com/js/js_arrays.asp)

[https://en . Wikipedia . org/wiki/Camel _ case # # target text = Camel %20 case % 20(风格化%20as%20camelCase，中间没有% 20 空格% 20 或% 20 标点符号。](https://en.wikipedia.org/wiki/Camel_case##targetText=Camel%20case%20(stylized%20as%20camelCase,no%20intervening%20spaces%20or%20punctuation.)

[https://youtu.be/oigfaZ5ApsM](https://youtu.be/oigfaZ5ApsM)

[https://developer . Mozilla . org/en-US/docs/Web/JavaScript/Reference/Global _ Objects/Array #](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array#)