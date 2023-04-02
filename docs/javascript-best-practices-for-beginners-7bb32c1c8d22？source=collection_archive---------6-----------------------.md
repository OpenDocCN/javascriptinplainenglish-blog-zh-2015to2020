# 面向初学者的 JavaScript 最佳实践

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-for-beginners-7bb32c1c8d22?source=collection_archive---------6----------------------->

![](img/1067eab6d774f80eb4af12410a761721.png)

Photo by [Todd Quackenbush](https://unsplash.com/@toddquackenbush?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何类型的应用程序一样，JavaScript 应用程序也必须写得很好。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将研究一些在编写 JavaScript 代码时应该遵循的最佳实践。

# 用===代替==

`===`在进行比较之前不进行数据类型强制。

自动类型强制的规则很复杂，所以如果我们依赖它，就会遇到问题。

因此，我们应该使用`===`而不是`==`进行等式比较。

同样，出于同样的原因，我们应该使用`!==`而不是`!=`进行不等式比较。

# 评估不好

永远不要在我们的 JavaScript 代码中使用`eval`。

它让我们从字符串中运行 JavaScript 代码。

这肯定是一个安全问题，JavaScript 引擎不能优化字符串中的代码，所以性能会受到影响。

也不可能轻松调试字符串中的代码。

# 不要用一些人手不够的人

有些缩写令人困惑，比如省略了单行代码块的大括号。

例如，我们可能有这样的东西:

```
if (foo)
   x = false
   bar();
```

我们可以假设两条线都是`if`模块的一部分。

但是只有第一行是块的一部分。

因此，我们应该使用花括号来分隔块:

```
if (foo) {
   x = false;
   bar();
}
```

把它们放进去，我们就永远清楚了。

# 利用 ESLint

我们应该使用 ESLint 来发现我们代码中的问题，并使风格保持一致。

这样，我们就不会有不一致的问题。

此外，它还内置了对检查 Node.js 代码的支持。

它还可以通过插件来保护代码、打字稿、JSX 等。

# 将脚本放在我们页面的底部

我们应该把脚本放在页面的底部，这样它们可以在页面内容加载后加载。

这对用户来说会让事情进展得更快。

例如，我们写道:

```
...
  <p>hello world</p>
  <script src="/file.js"></script>
  <script src="/anotherFile.js"></script>
</body>
</html>
```

让脚本持续加载。

# 在 For 语句之外声明变量

在 for 循环中，我们可以在`for`语句之外声明变量，这样它们就不会在每次迭代时都被声明。

例如，不要写:

```
for (let i = 0; i < someArray.length; i++) {
   let container = document.getElementById('container');
   container.innerHtml += `item ${i}`;
   console.log(i);
}
```

我们写道:

```
let container = document.getElementById('container');
for (let i = 0; i < someArray.length; i++) {
   container.innerHtml += `item ${i}`;
   console.log(i);
}
```

我们将`container`变量的定义移出，这样我们就不会在每次迭代时都声明它们。

# 构建字符串的最快方法

我们可以用 array 实例的`join`方法从一串字符串中更快更短地构建一个字符串。

例如，我们可以写:

```
const arr = ['item 1', 'item 2', 'item 3'];
const arr.join(', ');
```

用逗号作为分隔符将字符串组合在一起。

# 减少全局变量

我们不应该使用全局变量。

它们很容易被任何东西覆盖。

比如，代替写作；

```
var name = 'james';
var lastName = 'smith';

function doSomething() {...}
```

我们写道:

```
const person = {
   name: 'james',
   lastName: 'smith',
   doSomething() {...}
}
```

将项目放入对象中。

# 注释我们的代码

如果注释不重复代码，我们可以在代码上添加注释。

对于我们可能忘记的事情，它也很方便。

# 拥抱渐进增强

理想情况下，即使用户的浏览器没有启用 JavaScript，我们的性能也会下降。

如果用户禁用了 JavaScript，我们可以在 HTML 代码中放置一个`noscript`标签来显示一些内容。

如果启用了 JavaScript，那么我们可以显示常规内容。

# 不要将字符串传递给“setInterval”或“setTimeOut”

我们不应该将字符串传递给`setInterval`或`setTimeout`。

运行字符串比较慢，因为它们不能被优化。

此外，它们带来了安全风险，因为我们可以传入任何东西并尝试运行它。

调试字符串非常困难。

所以与其写:

```
setInterval(
"document.getElementById('container').innerHTML += `number: ${i}`", 3000
);
```

我们写道:

```
setInterval(() => {
  document.getElementById('container').innerHTML += `number: ${i}`
}, 3000);
```

# 不要使用“With”语句

千万不要使用`with`语句。

这应该是访问深层嵌套对象的一种简便方法。

然而，范围是混乱的。

所以与其写:

```
with (person.has.bodyparts) {
  arms = true;
  legs = true;
}
```

我们写道:

```
person.has.bodyparts.arms = true;
person.has.bodyparts.legs= true;
```

`with`在严格模式下也被禁用，所以我们肯定不能在大多数地方使用它。

我们也可以写:

```
let o = person.has.bodyparts;
o.arms = true;
o.legs = true;
```

![](img/27743c93996a7bcf429514ac20ee9057.png)

Photo by [Ilnur Kalimullin](https://unsplash.com/@kalimullin?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们不应该使用像`with`这样的旧结构，也不应该将字符串传递给`setTimeout`或`setInterval`。

还有，要用`===`和`!==`来比较事物。

还应该避免全局变量。

## 简单英语的 JavaScript

喜欢这篇文章吗？如果有，通过 [**订阅解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**