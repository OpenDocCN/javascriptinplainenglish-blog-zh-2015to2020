# ES2019(ES10)中的 JavaScript 新功能

> 原文：<https://javascript.plainenglish.io/javascript-es2019-es10-in-a-nutshell-cae6f7524519?source=collection_archive---------1----------------------->

## 简短、有用的 JavaScript 课程——让它变得简单。

![](img/a92258aa41470d9bebf40f21338058a7.png)

ECMAScript 规范的 2019 版有许多新功能。其中，我将总结那些对我来说最有用的。

首先，您可以在**节点运行这些示例，js ≥12** 。要在 Ubuntu-Debian-Mint 上安装 Node.js 12，您可以执行以下操作:

```
$sudo apt update
$sudo apt -y upgrade
$sudo apt update
$sudo apt -y install curl dirmngr apt-transport-https lsb-release  ca-certificates
$curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
$sudo apt -y install nodejs
```

或者，在 **Chrome Version ≥72、**中，您可以在开发者控制台中尝试这些功能(Alt +j)。

# array . prototype . flat & & array . prototype . flat map

**flat()** 方法创建一个新的数组，所有子数组元素递归连接到该数组中，直到指定的深度。

```
let array1 = ['a','b', [1, 2, 3]];
let array2 = array1.flat();
//['a', 'b', 1, 2, 3]
```

我们还应该注意，该方法排除了数组中的间隙或空元素:

```
let array1 = ['a', 'b', , , , 'c'];
let array2 = array1.flat();
// ['a','b','c']
```

flatMap()方法的效果与使用 Map()方法后跟 flat()方法的效果相同，默认深度为 1

```
let array1 = [1, 2, 3, 4];array1.flatMap(x => [x + 1]);
// [2, 3, 4, 5]
```

# 对象. fromEntries()

它创建一个对象或将键值对转换成一个对象。它只接受[项](https://alligator.io/js/iterables/)。

```
const obj = { a: 1, b: 2, c: 3};const entries = Object.entries(obj);
// entries
(3) [Array(2), Array(2), Array(2)]
0: (2) ["a", 1]
1: (2) ["b", 2]
2: (2) ["c", 3]const obj2 = Object.fromEntries(entries)
// {a: 1, b: 2, c: 3}
```

# string . trim start()& string . trimend()

trims start()/trimEnd()方法删除字符串开头/结尾的空白。

```
let result = '      hello!'.trimStart();
// "hello!"let cadena = 'hello      '.trimEnd()
// "hello!"
```

# 可选锁扣

它允许我们使用 try /catch，而无需在 catch 块中提供错误参数。

*无可选锁扣:*

```
try {
    // do something
} catch (e) {
    // we have error parameter to handle it
}
```

*带可选锁扣:*

```
try {
    // do something
} catch {
    // no binding or parameter to handle it
}
```

虽然在大多数情况下不建议这样做，但是当您知道您不打算使用异常对象时，这样做是有意义的。

# 函数“toString”修订版

现在，toString 方法必须返回从第一个标记开始到最后一个标记结束的源文本片段，并匹配适当的语法。

```
function /* kk*/ foo() { /* cc */ }// before:
foo.toString();
// "function foo() {}"// Now:
foo.toString();
//"function /* kk*/ foo() { /* cc */ }"
```

# 数组。排序稳定性

以前，V8 对超过 10 个元素的数组使用不稳定的快速排序。V8(Chrome ≥70)采用稳定的 TimSort 算法。

```
const array1 = [
 { name: "a",   age: 14 },
 { name: "b", age: 14 },
 { name: "c",  age: 13 },
 { name: "d",   age: 13 },
 { name: "e",   age: 13 },
 { name: "f",    age: 13 },
 { name: "g",   age: 13 },
 { name: "h",  age: 13 },
 { name: "i",   age: 12 },
 { name: "j",   age: 12 },
 { name: "k",    age: 12 }
]**const array2 = array1.sort( (a,b) => b.age - a.age)**
```

数组按名称的字母顺序预先排序。为了按年龄排序，我们传递了一个比较年龄的自定义回调。这是您所期望的结果:

```
**0: {name: "a", age: 14}
1: {name: "b", age: 14}**
2: {name: "c", age: 13}
3: {name: "d", age: 13}
4: {name: "e", age: 13}
5: {name: "f", age: 13}
6: {name: "g", age: 13}
7: {name: "h", age: 13}
8: {name: "i", age: 12}
9: {name: "j", age: 12}
10: {name: "k", age: 12}
```

数组是按年龄排序的，但是在每个年龄**内，它们仍然按名称**的字母顺序排序。

以前，JavaScript 规范不要求 Array.sort 的排序稳定性，而是将其留给实现。正因为如此，您也可以得到这样的结果，其中“b”出现在“a”之前:

```
**0: {name: "b", age: 14}
1: {name: "a", age: 14}**
2: {name: "c", age: 13}
3: {name: "d", age: 13}
4: {name: "e", age: 13}
5: {name: "f", age: 13}
6: {name: "g", age: 13}
7: {name: "h", age: 13}
8: {name: "i", age: 12}
9: {name: "j", age: 12}
10: {name: "k", age: 12}
```

# 符号.描述

符号是唯一标识符的内置数据类型。现在添加了一个新的属性 Symbol.prototype.description，以便从 Symbol 中获取描述。

以前

```
const symbol1 = Symbol('my symbol');
console.log(symbol1.toString());//Symbol(my symbol)
```

现在:

```
const symbol1 = Symbol('my symbol'); 
console.log(symbol1) 
console.log(String(symbol1) === `Symbol(${'my symbol'})`);
console.log(symbol1.description);//Symbol(my symbol)
//true
//my symbol
```

更多在[**JavaScript 2020 中的新特性**](https://medium.com/@kesk/new-javascript-features-in-es2020-c2d76acf9c5a)

## 非常感谢你阅读我！保重！