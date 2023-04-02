# JavaScript 错误——笨拙而无用的表达式

> 原文：<https://javascript.plainenglish.io/javascript-mistakes-clumsy-and-useless-expressions-733a61bec51b?source=collection_archive---------5----------------------->

![](img/921facb0cd0662fdfb14675c53079763.png)

Photo by [Sid Balachandran](https://unsplash.com/@itookthose?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是一种非常宽容的语言。编写可以运行但有错误的代码很容易。

在本文中，我们将看到一些不应该出现在 JavaScript 代码中的笨拙或无用的表达式，包括无用的块、幻数、多行字符串和空格，以及在没有分配返回对象的情况下使用`new`。

# 不必要的嵌套块

从 ES2015 开始，我们可以定义块来隔离我们的代码。它们用花括号表示。

这对于分离用`let`或`const`声明的块范围的变量或者以严格模式声明的类声明或函数声明很有用，

但是，我们应该不必要地添加块。例如，以下是无用的:

```
{
  function foo() {}
}
```

我们不应该把函数声明放在块中。这在 JavaScript 中是不允许的，因为它们只允许在顶层使用。

我们应该将函数声明放在顶层，如下所示:

```
function foo() {}
```

我们应该把用`let`和`const`声明的变量和常量放在一个块中，如下所示:

```
{
  let x = 1;
  const y = 2;
}
```

在上面的代码中，我们将`x`和`y`放在块中。这样，它们只能在块内部被访问。

# 没有神奇的数字

幻数是在我们的代码中多次出现的数字，没有任何明确的含义。它们应该被命名为常量，以使它们的含义清晰，并且我们可以在多个地方引用该常量。

这样，如果我们需要更改值，我们只需更改一次，而不是在多个地方进行更改。

例如，不写:

```
let x = 1;
let y = 1;
let z = 1;
```

我们可以将 1 移入一个常数，如下所示:

```
const numPerson = 1;
let x = numPerson;
let y = numPerson;
let z = numPerson;
```

这样，我们知道 1 是人数，我们也可以在多个地方重用它，如果我们需要改变它，只在一个地方改变它。

# 没有多个空格

多个空间令人困惑，而且不太干净。例如，代替书写:

```
if(foo  === "baz") {}
```

我们应该改为写:

```
if(foo === "baz") {}
```

但是，我们可以在行尾的多个空格后添加注释。例如，以下内容:

```
if(foo === "baz") {}    // check if foo is 'baz'
```

如果它能让注释排列得更好，那么注释出现在一段代码后的多个空格之后也是可以的。

# 没有多行字符串

如果我们想在拥有模板字符串之前编写多行字符串，我们必须编写如下内容:

```
let x = "foo \
         bar";
```

在上面的代码中，我们将`'bar'`放在带有`\`的新行上。这是 JavaScript 的一个未记录的特性，现在我们有了模板字符串，我们不应该使用它。

相反，我们应该使用如下的模板字符串:

```
let x = `foo 
bar`;
```

或者我们可以写:

```
let x = 'foo\nbar'
```

模板字符串保留所有的空格，所以我们必须精确地使用空格，

`\n`字符也创建了一个新行。

![](img/a1756c9383af79f6e3254fea59d2fa6c.png)

Photo by [Dan Gold](https://unsplash.com/@danielcgold?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 不要在没有指定返回对象的情况下使用 new 运算符

`new`操作符用于创建一个构造函数的新实例。所以，我们总要把它赋给某样东西。

使用`new`而不将返回的结果赋给任何东西是可疑的，因为要么它什么也没实现，要么我们用它来产生副作用。

构造函数不是提交副作用的好地方，因为大多数人期望它返回构造函数的实例。

因此，我们不应该在没有将返回值赋给我们选择的变量或常量的情况下使用`new`操作符。

例如，不写:

```
newFoo();
```

我们应该写:

```
const foo = new Foo();
```

# 不要使用函数构造函数

`Function`构造函数不好。我们传入字符串来指定参数和函数体。最后一个参数是主体，而它之前的参数是参数。

它们很难调试，因为它们都是字符串。因为它们都是字符串，所以也很难阅读。

此外，如果它们是动态的，那么攻击者可以注入他们自己的代码，创建他们自己的函数并运行恶意代码。

因此，我们应该不惜一切代价避免使用`Function`构造函数。而不是写:

```
const add = new Function("a", "b", "return a + b");
```

我们应该写:

```
const add = (a, b) => a + b;
```

这样，它们就很干净，很容易调试。

# 结论

无用的块、多余的空格、不标准的多行字符串都应该避免。

幻数应该被指定为常量，这样它们就可以被理解和重用。

用`new`返回的任何东西都应该赋给一个变量或常量。

此外，我们永远不应该使用`Function`构造函数来使我们的代码易于阅读和调试，并让我们避免潜在的安全问题。

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)，[**UX in Plain English**](https://medium.com/ux-in-plain-english)，[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，使用您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****