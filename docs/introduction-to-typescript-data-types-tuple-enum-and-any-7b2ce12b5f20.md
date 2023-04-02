# TypeScript 数据类型简介—元组、枚举和任意

> 原文：<https://javascript.plainenglish.io/introduction-to-typescript-data-types-tuple-enum-and-any-7b2ce12b5f20?source=collection_archive---------2----------------------->

![](img/da41df33d0adb9e3775975a022541760.png)

Photo by [Matt Nelson](https://unsplash.com/@mnelson?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 和其他编程语言一样，有自己的数据结构和类型。为了用 JavaScript 构建程序，我们必须了解一些数据类型。不同的数据可以放在一起构建更复杂的数据结构。

JavaScript 是一种松散类型或动态类型的语言。这意味着用一种类型声明的变量可以转换成另一种类型，而不用显式地将数据转换成另一种类型。

变量也可以在任何时候包含任何类型，这取决于赋值的内容。在动态类型语言中，如果不记录就很难确定变量的类型，我们可能会在变量中分配我们不想要的数据。

TypeScript 纠正了这些问题，让我们为变量设置固定的类型，这样我们就可以确定类型。在本文中，我们将研究 TypeScript 独有的 TypeScript 数据类型。在本文中，我们将研究 tuple、enum 和`any`数据类型。

# 元组

元组是逗号分隔的对象列表。我们可以有尽可能多的逗号分隔的项目。然而，在现实中，我们可能不应该在一个类型中有超过 10 个逗号分隔的项。在 TypeScript 中，我们可以使用括号声明类型为的变量，括号内的类型名称用逗号分隔。这意味着元组中的每个条目都将具有我们声明元组变量时设置的类型。例如，我们可以写:

```
let x: [string, number, boolean] = ['hello', 1, true];
console.log(x);
```

然后我们得到:

```
["hello", 1, true]
```

元组只是一个数组，每个条目都有固定的类型。如果我们在声明它的时候，把不同于我们指定的类型放在位置上，那么我们会得到一个错误。例如，如果我们有:

```
let x: [string, number, boolean] = [2, 1, true];
console.log(x);
```

然后我们得到“类型‘数字’不能赋给类型‘字符串’。”错误，程序将不会运行。我们可以像访问数组一样访问元组中的一个条目，因为它们只是每个条目都有固定类型的数组。例如，我们可以编写以下代码:

```
let x: [string, number, boolean] = ['hello', 1, true];
console.log(x);
console.log(x[0]);
console.log(x[1]);
console.log(x[2]);
```

然后我们得到:

```
hello
1
true
```

同样，析构赋值操作符也像其他数组一样正常工作。例如，我们可以写:

```
const x: [string, number, boolean] = ['hello', 1, true];
const [str, num, bool] = x;
console.log(x);
console.log(str);
console.log(num);
console.log(bool);
```

然后我们得到和以前一样的输出。我们也可以将非原语对象放在元组对象中。例如，我们可以有一个类的实例，如下面的代码所示:

```
class Person{
  name: string;
  constructor(name: string){
    this.name = name;
  }
}
const x: [string, number, boolean, Person] = ['hello', 1, true, new Person('Jane')];
const [str, num, bool, person] = x;
console.log(x);
console.log(str);
console.log(num);
console.log(bool);
console.log(person);
```

然后我们得到以下结果:

```
hello
1
true
Person {name: "Jane"}
```

从`console.log`输出。

# 列举型别

TypeScript 具有 JavaScript 中不可用的枚举类型。枚举类型是一种数据类型，它有一组命名值，称为该类型的元素、成员、枚举数或枚举数。它们是标识符，就像语言中的常量一样。在 TypeScript 中，枚举有一个与之关联的相应索引。默认情况下，成员从索引 0 开始，但可以更改为从我们喜欢的任何索引开始，后续成员的索引将从该起始编号开始递增。例如，我们可以编写以下代码，在 TypeScript 中定义一个简单的枚举:

```
enum Fruit { Orange, Apple, Grape };
```

然后我们可以用下面的代码访问一个枚举的成员:

```
enum Fruit { Orange, Apple, Grape };
console.log(Fruit.Orange);
```

那么上面代码中的`console.log`应该得到 0，因为我们没有为枚举指定起始索引。我们可以用类似下面的代码指定枚举的起始索引:

```
enum Fruit { Orange = 1, Apple, Grape };
console.log(Fruit.Orange);
console.log(Fruit.Apple);
console.log(Fruit.Grape);
```

然后，我们从每个`console.log`语句中按顺序记录以下内容:

```
1
2
3
```

我们可以为每个成员指定相同的索引，但这不是很有用:

```
enum Fruit { Orange = 1, Apple = 1, Grape };
console.log(Fruit.Orange);
console.log(Fruit.Apple);
console.log(Fruit.Grape);
```

然后我们得到:

```
1
1
2
```

来自`console.log`。正如我们所看到的，我们指定了索引，但是我们想改变它。我们甚至可以有负指数:

```
enum Fruit { Orange = -1, Apple, Grape };
console.log(Fruit.Orange);
console.log(Fruit.Apple);
console.log(Fruit.Grape);
```

然后我们得到:

```
-1
0
1
```

从`console.log`开始。要通过索引获取枚举成员，我们可以像通过索引访问数组条目一样使用括号符号。例如，我们可以编写以下代码:

```
enum Fruit { Orange, Apple, Grape };
console.log(Fruit[0]);
console.log(Fruit[1]);
console.log(Fruit[2]);
```

然后我们得到:

```
Orange
Apple
Grape
```

![](img/5e7c76dab708952c4e9ca9c5bfab1116.png)

Photo by [Anoir Chafik](https://unsplash.com/@anoirchafik?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 任何的

在 TypeScript 中，`any`类型意味着我们可以将任何东西赋给用类型`any`声明的变量。这和我们在 JavaScript 中给变量赋值没有什么不同。这让我们可以慢慢地采用 JavaScript 来打字稿，也让我们可以像字典一样使用动态对象。它还允许我们使用我们不知道来自第三方库模块的 like 成员类型的变量。我们可以将任何东西赋给一个`any`类型的变量，而不会出现任何错误。例如，我们可以像下面的代码一样声明并使用一个类型为`any`的变量:

```
let x: any = 1;
console.log(x);
x = 'string';
console.log(x);
```

如果我们运行上面的代码，那么我们得到下面的`console.log`值:

```
1
string
```

类型对于声明其他数据类型也很方便，比如数组。如果我们用`any`类型声明一个数组，那么我们可以将任何类型的数据作为条目放入我们声明的数组中。我们可以用下面的代码声明一个类型为`any`的数组:

```
let anyList: any[] = [1, true, "abc"];
console.log(anyList);
```

然后我们得到:

```
[1, true, "abc"]
```

从`console.log`开始。TypeScript 有一个与 JavaScript 中的`Object`对象相对应的`Object`类型。所以不能像`any`型那样使用。`Object`式有自己的方法，如`toString`、`hasOwnProperty`等。，它与`any`类型完全不同，后者实际上意味着变量可以被赋予任何值。例如，如果我们编写以下内容:

```
let x: Object = 2;
x.toFixed();
```

我们将得到错误“类型“Object”上不存在属性“toFixed”。但是，我们可以编写以下代码:

```
let x: Object = 2;
console.log(x.hasOwnProperty('foo'));
```

我们可以看到，`Object`类型有一个`hasOwnProperty`属性方法，这就是 JavaScript 中的`Object`对象所具有的。

元组是逗号分隔的对象列表。我们可以有尽可能多的逗号分隔的项目。它只是一个 JavaScript 数组，每个条目都有固定的类型。TypeScript 具有 JavaScript 中不可用的枚举类型。枚举类型是一种数据类型，它有一组命名值，称为该类型的元素、成员、枚举数或枚举数。

它们是标识符，就像语言中的常量一样。每个枚举成员都有一个索引，可以任意设置。我们也可以用括号符号从成员的索引中获取成员名，就像我们如何通过索引获取数组条目一样。

`any`类型允许我们给用类型`any`声明的变量赋值。这和我们在 JavaScript 中给变量赋值没有什么不同。