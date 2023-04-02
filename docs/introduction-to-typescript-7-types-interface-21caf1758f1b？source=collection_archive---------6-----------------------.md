# TypeScript 简介(7 种类型和接口)

> 原文：<https://javascript.plainenglish.io/introduction-to-typescript-7-types-interface-21caf1758f1b?source=collection_archive---------6----------------------->

![](img/08767705c892b6370696d41561bcd2f8.png)

# 什么是静态类型语言？

**静态类型语言示例**

*   Java，C++

**动态类型语言示例**

*   Python，PHP

**动态类型语言**

*   写代码时不需要关心类型
*   小型项目的生产率更高
*   **运行时**出现类型错误

**静态类型语言**

*   需要注意为变量声明设置正确的类型
*   大型项目的生产率更高
*   类型错误发生在**编译时**

**换句话说，**

*   类型错误发生在*运行时*在**动态类型语言**中，而类型错误发生在*编译时*和**类型错误**在**编译时**是检测错误的安全方式，而不是在*实际运行代码*时发现错误

# 为什么是打字稿

*   Microsoft 支持 TypeScript，它比其他 JS 静态类型工具有更大的社区
*   在 vs code(MS 的另一个产品)中，Typescript 文件`.ts`进行自动类型检查

**JS 中静态类型的其他工具**

*   Elm，ReasonML，PureScript，Flow

# 打字稿网站

*   所有的代码都可以在官方打字网站[https://www.typescriptlang.org/play](https://www.typescriptlang.org/play)上立即运行

# 类型 1 >空且未定义的类型

JavaScript 中的`null` & `undefined`是 TypeScript 中的实际类型

**// 1 :** `undefined` & `null`可以在 TypeScript 中作为类型使用

**// 2 :** 将数字类型赋给`undefined`类型变量时类型错误

**// 3:** `undefined` & `null`类型通常被定义为联合类型(即 a | b:要么是 a 类型，要么是 b 类型)

# 类型 2 >数字和文字类型

**// 1 :** 数字 10，20，30 用作类型。只有 10、20、30 可以分配给变量`v1`

**// 2 :** 因此键入错误为`v1` 15！== 10 | 20 | 30

**// 3 :** 同样，v2 也有类型`policeman` | `firefighter`

# 类型 3 >任何类型

`any`类型允许所有类型

*   类型也可以有类型的功能
*   从 JS 到 TS 重构时，类型非常有用，因为一次为所有变量定义类型工作量太大(暂时防止**类型错误**)，但是应该避免误用`any`

# 类型 4 >无效和从不键入

**void 类型:**不返回任何东西的函数可以设置为`void`类型

**永不类型:**一个因异常、意外错误、无限期待而永不终止的函数，可以设置为`never`类型

**// 1 :** 设置为`void`类型，因为没有返回任何内容

**// 2:** 设置为`never`类型，因为函数因意外错误而终止

**// 3:** 设置为`never`类型，因为函数没有终止；无限循环

# 类型 5 >对象类型

**// 1 :** 对象`v`中没有`prop1`属性；因此，在访问对象`v`中的`prop1`属性时出现类型错误。需要使用**接口、**(后面会介绍)来定义带有属性信息的类型

# 类型 6 >相交和联合类型

交集类型用`&`表示，并集类型用`|`表示

**// 1 :** v1 类型同 3 | 5 = = =(1 |**3**|**5**)&(**3**|**5**| 7)

**// 2:** 因此，类型错误；v1 不能是 1 (v1: 3 | 5)

# 类型 7 >枚举类型

## `>> enum`类型可以是每个元素的**值**或**类型**

**// 1 :** 将`Fruit`定义为`enum`类型

**// 2:** 用作值(=) `Fruit = Fruit.Apple`

**// 3:** 用作类型(:)`v2: Fruit.Apple`

## `>> enum`键入数字值

**// 1 :** 0 自动赋给`enum`类型的第一个**元素**

**// 2 :** 可以给`enum`类型中的每个元素分配数字或字符串。如果没有明确指定任何值，则指定一个从前一个元素增加+1 的值(结果。橙色=== 6)

## `>> enum`键入字符串值

# 类型 8 >功能类型

## `>> parameter type & return type of function`

参数类型&返回类型是定义`function`类型所必需的

**// 1 :** *参数类型*:名称:字符串，年龄:数字& *返回类型*:字符串

**// 2 :** 可以使用`substr`方法，因为`name`是`string`类型

**// 3:** 可以比较`≥`因为`age`是`number`类型

**// 4 :** 类型错误，因为`getInfoText`函数期望其第二个参数为`number`类型

**// 5:** 类型错误，因为`getInfoText`函数的返回类型是`string`，所以不能赋值给`v3`，因为`v3`有`number`类型

## `>> optional parameter in a function`

`?:`代表可选参数

**// 1:** 语言作为可选参数；没有为参数位置提供值的类型错误

**// 2:** 因`language`为**可选**而导致不检查参数存在检查的类型错误

```
if (typeof variable !== 'undefined') {
    // the variable is defined
}
```

**// 3:** 语言参数可选

**// 4:** 需要提供正确的类型即`string`

# 连接

## `> define object type using interface`

**// 1 :** 定义`Person`接口

**// 2:** 为每个属性定义类型

**//3:`age`需要`number`类型时提供`string`类型的**类型错误

## `> optional type in Interface`

## `> readonly property`

`readonly`关键词

**// 1 :** 可以在定义变量`p1`时赋值

**// 2:** 变量初始定义后不能修改，因为`name`是只读属性

## `> assign value to not defined property`

**// 1:** 类型错误，因为`birthday`没有在`Person`接口中定义

**// 2:** 变量`p2`有一个属性没有在`Person`接口中定义

## `> index Type in interface`

**// 1:** 将`string`或`number`类型定义给所有以字符串为关键字的属性

**// 2:** `birthday`不会导致任何错误，因为`birthday`键是字符串，`birthday`值是字符串(字符串|数字)

**// 3:** `age`被明确定义为`number`类型所以类型错误

> 编码快乐！

## **用简单的英语写的 JavaScript 的注释:**

我们已经推出了三种新的出版物！请关注我们的新出版物，表达对它们的爱:[**AI in Plain English**](https://medium.com/ai-in-plain-english)，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**![](img/ae0cad5e6f08eb2b81958e1939cfd388.png)**

**Photo by [Simon Maage](https://unsplash.com/@simonmaage?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)**