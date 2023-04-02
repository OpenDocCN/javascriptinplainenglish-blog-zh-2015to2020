# JavaScript 最佳实践——糟糕的表达式和语句

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-simple-expressions-b2b104d66eb6?source=collection_archive---------11----------------------->

![](img/5ecf96b6ece10bcf797811eb804864ca.png)

Photo by [Martin Moreno](https://unsplash.com/@memoreno?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究一些在编写 JavaScript 代码时应该遵循的最佳实践。

# 当存在更简单的选择时，没有三元表达式

当存在更简单的选择时，我们不应该写三元表达式。

例如，如果我们有:

```
let score = val ? val : 0
```

那么我们应该写:

```
let score = val || 0
```

相反，因为他们做同样的事情。

如果`val`是 falsy，那么在两个例子中都将返回 0。

# 在 return、throw、continue 或 break 语句之后没有无法访问的代码

如果我们编写这些语句，那么我们应该确保在某些情况下它后面的代码仍然是可访问的。

例如，我们不应该有这样的代码:

```
function foo () {
  return true;
  console.log('never');
}
```

控制台日志不可访问，因为它出现在一个总是运行的`return`语句之后。

# finally 块中没有流控制语句

我们不应该在`finally`块中使用流控制语句。

JavaScript 挂起`try`和`catch`块的流控制语句，直到`finally`块的执行完成。

所以当`finally`中使用`return`、`throw`、`break`或`continue`时，`try`或`catch`中的控制流语句被覆盖。

因此，可能会导致意外的行为。

所以我们不应该写这样的代码:

```
try {
  // ...
} catch (e) {
  // ...
} finally {
  return 1;
}
```

# 关系运算符的左操作数不应被求反

如果我们有这样的代码:

```
if (!key in obj) {}
```

那么只有`key`被否定，这很可能是一个错误。

相反，我们应该否定整个表达式:

```
if (!(key in obj)) {}
```

# 避免不必要的呼叫和申请

我们只需要使用`call`和`apply`来改变函数内部`this`的值。

除此之外，我们可以使用 spread 操作符将数组条目扩展到参数中，而不是调用`apply`。

所以与其写:

```
sum.call(null, 1, 2, 3)
```

我们写道:

```
sum.call(1, 2, 3)
```

或者:

```
sum.call(...nums)
```

# 不要在对象上使用不必要的计算属性键

我们不应该在对象上使用不必要的计算属性键。

它们没有用，可以简化。

例如，不写:

```
const user = { ['name']: 'james' };
```

我们写道:

```
const user = { name: 'james' };
```

# 没有不必要的构造函数

如果我们的类中有一个空的构造函数，那么我们不需要它。

所以我们不应该写:

```
class Car {
  constructor () {      
  }
}
```

因为 JavaScript 将为我们提供一个空的构造函数。

# 没有不必要的转义字符的使用

我们不应该用无用的方式来转义字符。

因此，我们不应该编写这样的代码:

```
let message = 'hell\o'
```

# 不要将导入、导出和析构的赋值重命名为相同的名称

将导入、导出和重构的赋值重命名为相同的名称是没有意义的。

因此，我们不应该这样做。

例如，不写:

```
import { foo as foo } from './config'
```

相反，我们只是写:

```
import { foo } from './config'
```

# 属性前没有空格

属性前的空格是无用的，也不符合普遍接受的 JavaScript 代码格式约定。

因此，我们不应该拥有它们。

例如，不写:

```
user .password
```

我们写道:

```
user.password
```

# 没有语句

我们不应该使用`with`语句。

在严格模式下不允许使用它们。

此外，它们会创建具有混淆范围的变量。

因此，我们不应该写这样的东西:

```
with (obj) {...}
```

# 对象属性之间的换行符保持一致

当我们定义对象文字时，我们应该在每个属性后换行。

或者，如果不溢出页面，我们可以在同一行中输入所有属性。

例如，我们要么写:

```
const user = { name: 'jame', age: 30, password: '123' }
```

或者我们写:

```
const user = { 
  name: 'jame', 
  age: 30, 
  password: '123' 
}
```

# 块内无填充

我们不应该在块内填充。

例如，我们不应该写:

```
if (user) {

  const name = getName()

}
```

相反，我们写道:

```
if (user) {
  const name = getName()
}
```

![](img/e6778bacc848638e2bad81d849c5c8f8.png)

Photo by [John Mccann](https://unsplash.com/@jmacca88?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们不应该有无用的空白。

此外，当涉及到换行符时，我们应该格式化对象的一致性。

我们还应该确保我们的块中没有任何不可到达的代码。

# **简明英语 JavaScript**

你知道我们有四份出版物和一个 YouTube 频道吗？在 [**plainenglish.io**](https://plainenglish.io/) 和 [**找到它们订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**