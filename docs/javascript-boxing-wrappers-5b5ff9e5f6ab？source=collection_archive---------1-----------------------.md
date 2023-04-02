# JavaScript 拳击包装器

> 原文：<https://javascript.plainenglish.io/javascript-boxing-wrappers-5b5ff9e5f6ab?source=collection_archive---------1----------------------->

## 简短、有用的 JavaScript 课程—让它变得简单

![](img/8253d2768ab9dfd53733926545cb2214.png)

By undraw.com

在 JavaScript 和其他语言中，基元值没有方法或属性，所以如果您想使用它们，您需要使用包装器。

原语是编程语言中最简单的元素。JavaScript 有六种基本类型:字符串、数字、布尔、空、未定义和符号，其他都是对象。

# 索引

*   自动装箱
*   手动拳击和陷阱
*   取消装箱
*   结论

# 自动装箱

装箱是将一个原始值包装在一个对象中。当你像对待一个对象一样对待一个基本类型时，例如，调用 toLowerCase 函数，JavaScript 会将基本类型包装到相应的对象中。这个新的对象然后被链接到相关的内置<.prototype>上，所以你可以在基本类型上使用原型方法。

正如您在这里看到的，当您试图对一个基元值使用一个方法时，JavaScript 会自动进行装箱，因此，您可以使用 String 对象的不同方法:

```
//String primitive name
const name = "KESK";name.toLowerCase();//kesk
name.substring(1);//ESK
```

自动装箱是一个重要的特性，因为它使标准库方法易于访问。

# 手动拳击和陷阱

一般来说，直接使用装箱的对象包装器通常不是一个好主意，因为有些问题与他有关，如果您不想得到意外的结果，就必须小心。

考虑以下示例:

```
const a = new Boolean(true)
if(a) console.log("it's true")// it's trueconst b = new Boolean(false)
if(!b) console.log("never runs");// *objects are “truthy.“*const c = Object(false)
if(!c) console.log("never runs"); // *objects are “truthy.“*
```

这里的问题是，您正在为一个错误的值**创建一个对象包装器，但是对象是“真实的”。**所以，如果你想手工输入原始值，请小心。

```
const b = Boolean(false)
if(!b.valueOf()) console.log("its false"); // its false
```

一般来说，最好让隐式装箱，因为浏览器是为此而优化的，所以如果您尝试手动装箱，您的代码可能会变慢。而且，当直接使用对象包装器时很容易出错。喜欢使用基本值，例如，字符串“abc”与等效的包装对象。

# 取消装箱

从对象包装器获取基础基元值的最简单方法是使用 valueOf()方法:

```
const a = Object(false);
a == false; //true
a === false //falsea.valueOf() == false //true
a.valueOf() === false //true
```

取消装箱也可以隐式完成(强制):

```
a == false; //true
```

# 结论

装箱和拆箱是 T4 的常规做法，但是根据语言和实现的不同，包装器的使用会比仅仅使用原始值更慢，占用更多的内存。另一方面，它允许您使用更高级别的数据结构或方法，并在代码中实现更高的灵活性。

如果您使用手动拳击，请小心包装陷阱；此外，一般来说，最好让隐式装箱，因为浏览器引擎是为此而优化的。

谢谢你看我！

## **简单英语 JavaScript**

你知道我们有三个出版物和一个 YouTube 频道吗？在 [**找到所有链接！**](https://plainenglish.io/)