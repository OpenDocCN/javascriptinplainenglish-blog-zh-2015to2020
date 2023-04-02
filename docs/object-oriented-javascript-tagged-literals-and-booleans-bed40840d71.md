# 面向对象的 JavaScript——标记文字和布尔值

> 原文：<https://javascript.plainenglish.io/object-oriented-javascript-tagged-literals-and-booleans-bed40840d71?source=collection_archive---------2----------------------->

![](img/999da0c3100a749b9444e6bd3b50b8bd.png)

Photo by [Brett Jordan](https://unsplash.com/@brett_jordan?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 部分是面向对象的语言。

要学习 JavaScript，我们必须学习 JavaScript 的面向对象部分。

在本文中，我们将研究字符串和布尔值，它们是对象的构造块。

# 标记的模板文字

标记模板文字是另一种类型的字符串文字。

它们允许用函数修改模板文字的输出。

通过在模板文本前面加上函数来调用函数。

例如，我们可以写:

```
function transform(strings, ...substitutes) {
  console.log(strings);
  console.log(substitutes);
}const firstname = "James";
const lastname = "Bond"
transform `Name is ${firstname} ${lastname}`;
```

`transform`是模板标签，它是一个采用一组特定参数的函数。

`strings`是字符串中不在花括号中的部分的数组。

`substitutes`具有花括号中的值。

所以`strings`是:

```
["Name is ", " ", ""]
```

而`substitutes`是”

```
["James", "Bond"]
```

# 布尔运算

布尔值只能取两个值中的一个。

可以是`true`也可以是`false`。

例如，如果我们有:

```
let b = true;
```

然后:

```
typeof b;
```

返回`'boolean'`。

如果我们把`true`或`false`放在引号中，那么它们就是字符串。

所以如果我们有:

```
var b = "true";
typeof b;
```

那么`typeof b`就是`'string'`。

# 逻辑运算符

3 运算符处理布尔值。

`!`是逻辑非运算符。

`&&`是逻辑与运算符。

而`||`是逻辑 OR 运算符。

`!`对布尔值求反。所以如果我们有:

```
let b = !true;
```

那么`b`就是`false`。

如果我们使用逻辑 NOT 两次，那么我们得到原始值。如果我们有:

```
let b = !!true;
```

那么`b`就是`true`。

如果对非布尔值使用逻辑运算符，则该值将转换为布尔值。

如果我们有:

```
let b = "foo";
```

那么`!b;`就是`false`。

如果我们有双重否定:

```
let b = "one";
!!b;
```

然后我们得到`true`。

大多数值都用双重否定运算符转换成`true`,但以下值除外:

*   空弦`“”`
*   `null`
*   `undefined`
*   `0`
*   `NaN`
*   `false`

上面的值都是假的。

当两个操作数都是`true`时，`&&`运算符返回`true`。否则返回`false`。

当任一操作数为`true`时，`||`运算符返回`true`。否则返回`false`。

我们可以依次使用多个操作符。

例如，我们可以写:

```
true && true && false && true;
```

我们得到了`false`。

并且:

```
false || true || false;
```

我们得到了`true`。

如果我们在一个表达式中混合了`||`和`&&`，那么我们应该添加括号。

例如，我们可以写:

```
false && (false || true) && true;
```

我们得到`false`。

# 运算符优先级

运算符具有优先权。

算术遵循与数学相同的规则。

所以如果我们有:

```
1 + 2 * 3;
```

我们得到 7，因为乘法在前面。

对于逻辑运算符，`!`具有最高的优先级，它首先运行。

然后是`&&`和`||`。

所以:

```
false && false || true && true;
```

与以下内容相同:

```
(false && false) || (true && true);
```

![](img/509304d2125ef1a3e789d4a031541645.png)

Photo by [Mnz](https://unsplash.com/@mnzoutfits?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们应该注意运算符的优先级。

此外，有几个操作符我们应该知道。

## **用简单英语写的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**