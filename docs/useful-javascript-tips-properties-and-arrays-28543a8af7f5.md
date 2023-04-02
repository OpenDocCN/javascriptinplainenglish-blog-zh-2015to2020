# 有用的 JavaScript 技巧—属性和数组

> 原文：<https://javascript.plainenglish.io/useful-javascript-tips-properties-and-arrays-28543a8af7f5?source=collection_archive---------8----------------------->

![](img/ed3dac75df0b96fb63e0493436f6bf02.png)

Photo by [Srecko Skrobic](https://unsplash.com/@sreckoskrobic?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将了解一些应该遵循的技巧，以便更快更好地编写 JavaScript 代码。

# 检查属性是否在对象中

有几种方法可以用来检查一个属性是否在一个对象中。

例如，我们可以如下使用`hasOwnProperty`方法:

```
obj.hasOwnProperty('name');
```

`hasOwnProperty`是从`Object.prototype`继承的对象的方法。

如果`'name'`是对象/的非继承属性，则返回`true`

同样，我们可以使用`in`操作符来检查一个属性是否是对象的一部分，包括在原型中。

例如，如果我们写:

```
'valueOf' in obj;
```

那么它将返回`true`，因为`valueOf`是从`obj`的原型继承的。

# 模板字符串

模板字符串是将各种表达式组合成一个字符串的绝佳形式。

我们可以使用模板字符串来代替连接。

例如，不写:

```
const firstName = 'jane';
const lastName = 'smith';
const name = firstName + ' ' + lastName;
```

我们可以写:

```
const firstName = 'jane';
const lastName = 'smith';
const name = `${firstName} ${lastName}`;
```

它更短，而且没有加号。

# 将节点列表转换为数组

我们可以使用 spread 操作符将节点列表转换为数组。

例如，我们可以写:

```
const nodeList = document.querySelectorAll('div');
const nodeArr = [...nodeList];
```

我们可以这样做，因为节点列表是一个类似数组的可迭代对象，这意味着它可以用 spread 操作符转换为数组。

现在我们可以在节点列表上使用任何数组方法，这让我们的生活变得更加容易。

# 编写一个既能处理数组又能处理单个值的方法

我们可以写一个方法，通过检查一个数组是否是一个数组来处理两个数组和一个值。

例如，我们可以写:

```
const foo = (val) => {
  if (Array.isArray(val)){
    //...
  }
  else {
    //...
  }
}
```

我们使用了`Array.isArray`方法来检查`val`是否是一个数组。

然后我们可以有一个函数，既可以处理数组，也可以处理单个值。

# 提升

我们应该注意函数声明的提升。

提升意味着值被拉到文件的顶部。

函数声明被挂起。

所以如果我们有:

```
function foo(){
  //...
}
```

然后可以在文件中的任何地方调用它。

# 对带有重音字符的字符串进行排序

为了对带有重音字符的字符串进行排序，我们可以使用`localeCompare`方法对它们进行适当的排序。

例如，我们可以写:

```
['marrón', 'árbol', 'dulce', 'fútbol'].sort((a, b) => a.localeCompare(b));
```

然后我们可以毫无争议地对它们进行分类。

我们得到:

```
["árbol", "dulce", "fútbol", "marrón"]
```

这正是我们所期待的。

同样，我们可以使用如下的`Intl.Collator`方法来做同样的事情:

```
['marrón', 'árbol', 'dulce', 'fútbol'].sort(Intl.Collator().compare);
```

它需要两个带有字符串的参数来比较并返回与`localeCompare`相同的东西，所以我们可以直接传递函数。

在比较大量字符串时比`localeCompare`更快。

因此，当我们比较非英语字符串时，我们应该使用这两个函数来比较它们。

# 改进嵌套条件句

我们可以通过使用`switch`语句来消除嵌套条件。

例如，不写:

```
if (color) {
  if (color === 'black') {
    doBlack();
  } else if (color === 'red') {
    doRed();
  } else if (color === 'blue') {
    doBlue();
  } else {
    doSomething();
  }
}
```

我们可以用`switch`语句来代替:

```
switch(color) {
  case 'black':
    doBlack();
    break;
  case 'red':
    doRed();
    break;
  case 'blue':
    doBlue();
    break;
  default:
    doSomething();
}
```

它更短，我们不需要在进行其他检查之前检查`color`是否为假。

这是因为`default`条款负责虚假检查。

# 在数组中添加项目

我们可以使用`concat`方法添加一个项目。

例如，我们可以写:

```
const arr = [1,2,3];
const four = arr.concat(4);
```

`concat`返回追加了新项目的数组。

所以我们用`[1, 2, 3, 4]`代替`four`。

我们可以使用`push`将一个条目添加到数组中。

例如，我们可以写:

```
const arr = [1,2,3];
arr.push(4);
```

`arr`现在是`[1, 2, 3, 4]`。

![](img/867f2b8cf7333c0fac2477e327786b31.png)

Photo by [Krisztina Papp](https://unsplash.com/@almapapi?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以使用`concat`或`push`向数组中添加条目。

同样，我们可以使用`in`或`hasOwnProperty`来检查一个属性是否在一个对象中。

如果我们对带有重音符号的字符串数组进行排序，那么我们需要使用`localeCompare`或`Intl.Collator().compare`。

## 简单英语的 JavaScript

你知道我们有四份出版物和一个 YouTube 频道吗？在[**plain English . io**](https://plainenglish.io/)和 [**找到它们订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**