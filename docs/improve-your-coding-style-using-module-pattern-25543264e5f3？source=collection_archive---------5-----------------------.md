# 使用模块模式改进您的编码风格

> 原文：<https://javascript.plainenglish.io/improve-your-coding-style-using-module-pattern-25543264e5f3?source=collection_archive---------5----------------------->

![](img/3041cf06874a5c87cec1c826b56a6f80.png)

Photo by [Alexandru Acea](https://unsplash.com/@alexacea?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 设计模式

设计模式通常用于解决软件开发中常见的问题。有人可能想知道为什么我们甚至需要设计模式？解决问题的方法有一百万种，但是既然问题已经解决了，我们为什么还要考虑去解决它呢？设计模式建立了解决问题的标准结构方法，这些方法已经被证明是有效的。

# 设计模式的类型

根据模式解决的问题类型，它们被分为三类:

## 创造模式

在实例化一个类时，也就是在创建一个对象的过程中，它处理不同的机制。

## 结构模式

在处理大型应用程序时，它处理多个类和对象的组织，以保持可伸缩性和效率。

## 行为模式

它处理任何两个对象之间通信过程中涉及的问题。

# 模块模式

模块模式是一种结构模式，它将相关的函数、类或单体组合成一个实体。它是最常用的 Javascript 模式之一，非常强大，因为它有助于构建通用代码。

模块模式定义了如何定义对象，以及如何在各自的作用域之外访问特定于某个类的函数。通过将一个方法定义为 public 或 private，我们可以控制它如何在范围之外公开，并且通过这样做，数据隐藏和数据抽象也可以在这个模式中实现。

下面是模块模式的一个简单实现:

```
function CarDetails() {
  var name = 'Chiron';
  var manufacturer = 'Bugatti';
  return {
    name: name,
    manufacturer: manufacturer
  }
}
var newCar = CarDetails();
var carName = newCar.name; // Chiron
var manufacturer = newCar.manufacturer; // Bugatti
```

上面的代码描述了汽车的细节是如何包装在一个函数中的。与新车相关的属性，如`name`或`manufacturer`，可以通过为该类创建一个对象来访问，因为它们都是公开的。在这里，与汽车相关的所有属性都被打包到一个名为`CarDetails`的模块中。

# 模块模式中的数据隐藏

检查下面的代码，它对外界隐藏了属性`price`和`tax`:

```
function CarDetails() {
  var name = 'Chiron';
  var manufacturer = 'Bugatti';
  var price = 190000000;
  var tax = 10;
  var totalPrice = function() {
    return price + (price * tax)/100;
  }
  return {
    name: name,
    manufacturer: manufacturer,
    totalPrice: totalPrice
  }
}
var newCar = CarDetails();
var carName = newCar.name; //Chiron
var manufacturer = newCar.manufacturer; // Bugatti
var totalPrice = newCar.totalPrice(); // 209000000
var tax = newCar.tax; // undefined
```

函数`totalPrice`帮助执行私有变量`price`和`tax`的操作，使它们保持私有，并且只在类`CarDetails`的范围内。这样我们可以防止私有对象和函数被公开。

## 立即调用函数表达式

实现模块模式最常见的方法之一是使用**life**(立即调用函数表达式)。它基本上是一个 Javascript 函数，一旦被定义就自动执行。

上述方法也可以写成:

```
var newCar = function CarDetails() {
  var name = 'Chiron';
  var manufacturer = 'Bugatti';
  var price = 190000000;
  var tax = 10;
  var totalPrice = function() {
    return price + (price * tax)/100;
  }
  return {
    name: name,
    manufacturer: manufacturer,
    totalPrice: totalPrice
  }
}()
var carName = newCar.name; // Chiron
var manufacturer = newCar.manufacturer; // Bugatti
var totalPrice = newCar.totalPrice(); // 209000000
```

这里，我们不是为`CarDetails`创建一个对象，而是自己调用一个函数定义，并将返回的对象保存为`newCar`。即使在这种情况下，与`CarDetails`相关的属性也可以通过返回的对象来访问。

# **模块模式变化**

## 导入混合

在 Javascript 中，可以将一个全局对象传递给一个匿名函数，让我们匿名传递全局参数，并以我们选择的名字使用它们。

下面是这种模块模式变体的实现示例:

```
var module = (function (_) {
  function privateMethod() {
    return _.isNumeric(1);
  }
  return{
    publicMethod: function(){
      return privateMethod();
    }
  };
})(jQuery);
module.publicMethod(); // true
```

## 柔道训练学校

这种模块模式的变体帮助我们用方法`dojo.setObject`更好地处理对象。使用`setObject()`,我们可以设置在路径中创建了一个中间对象的子对象的值，如果它不存在，就作为参数传递。

这里有一个描述`setObject`用法的例子:

```
require(['dojo/_base/lang'], function(lang){
  lang.setObject('parent.child.prop', 'some value', obj);
});
```

## 出口

模块模式基本上意味着 ***将代码分割成不同的模块*** 。在`export`的帮助下，我们可以定义某个功能并将其导出，如果需要，还可以将其导入其他模块。

考虑下面的例子，我们导出汽车列表:

```
const cars = ['VW', 'Bugatti', 'Maserati'];
const getCars = () => cars;
export { getCars };
```

每当需要汽车列表时，可以使用`import`导入导出的模块，如下所示:

```
import { getCars } from '..'; // from the other fileconsole.log(getCars()); // ['VW', 'Bugatti', 'Maserati']
```

# 优势

如果他们正在寻找以下内容，应该选择这种模式:

1.  干净的代码
2.  数据隐私
3.  显式定义的公共方法和变量。
4.  通过闭包实现函数和变量的本地化。
5.  它提供了结构
6.  避免污染全球范围

# 结论

模块基本上是一个自我维持的代码块，可以独立工作。我们可以轻松地控制任何对象的公开性，这使得模块模式成为 JavaScript 中最强大的模式。当我们导入 CSS 文件或任何其他 JavaScript 文件时，可以在使用`import`或`require`甚至使用`<script>`标签时找到模块模式的另一种常见用法。对于每个开发人员来说，理解模块模式是如何工作的是绝对重要的，因为这个模式涵盖了 OOPS 的两个主要原则——抽象和封装。模块模式无疑是帮助维护干净代码的最强有力的模式之一。它还促进了代码的重用和维护适当的结构。如果您的应用程序处理多个函数，最好采用这种模式，因为这将有助于降低复杂性。

***参考:*
【学习 JavaScript 设计模式】***Addy Osmani*著

感谢您的阅读！

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**