# 解释了零化合并运算符

> 原文：<https://javascript.plainenglish.io/nullish-coalescing-operator-explained-8b1e0dc6c24c?source=collection_archive---------6----------------------->

![](img/b6b94ed37627837bc7e1778ee7af7bee.png)

Photo by [Myburgh Roux](https://www.pexels.com/@myburgh?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) from [Pexels](https://www.pexels.com/photo/codes-computer-monitor-programming-1102797/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

Nullish 合并运算符是 javascript 中的一个新运算符，实际上是 ES2020 规范。它是一个短路操作符，很像`&&`和`||`操作符。

# 这是什么？

Nullish 合并运算符(`??`)是一个逻辑运算符，当 LHS 操作数为`nullish`(空或未定义)时，返回其 RHS 操作数，否则返回其 LHS 操作数。

```
const name = *null* ?? ‘John Doe’
*// output: John Doe*
```

# 我们为什么需要它？

在零化合并运算符之前，逻辑 OR ( `||`)运算符用于检查零化值。

```
const name = *null* || 'John Doe'*// output: John Doe*
```

但问题是`||`检查为`truthy`还是`falsy`值。也就是说，当 LHS 操作数为`falsy` (null，undefined，false，0，''，NaN)时，它返回 RHS 操作数，否则，它返回 LHS 操作数。

```
const name = '' || 'John Doe'*// output: John Doe*const name = '' ?? 'John Doe'*// output: ''*
```

为了更好地理解这个问题，请考虑下面的代码示例。

```
function joinArray(*array*, *delimiter*) { delimiter = delimiter || ','; return arr.join(delimiter);}
```

在本例中，`delimiter`参数是可选的。如果没有为分隔符参数传递值，则`,`将是默认值。但是这段代码有一个 bug。如果我们想要在没有任何分隔符的情况下加入数组，也就是使用`''`作为参数，会发生什么？

```
let joined = joinArray(['Find', 'the', 'bug'], '');*// expected output: Findthebug**// actual output: Find,the,bug*
```

如你所见，预期的输出是‘findtebug’，但实际的输出是 Find，the，bug’。这是因为`||`操作符只检查操作数是`truthy`还是`falsy`，空字符串(`''`)的计算结果为假。因此，返回 RHS 操作数，在本例中为逗号(`,`)。

我们可以通过用`??`替换`||`来轻松修复这个 bug。

```
function joinArray(*array*, *delimiter*) { delimiter = delimiter ?? ','; return arr.join(delimiter);}
```

我们得到了正确的结果。

```
let joined = joinArray(['Find', 'the', 'bug'], '');*// output: Findthebug*
```

# 关于用 AND 或 or 运算符链接的注释

不能用`??`直接链接 AND ( `&&`)和 OR ( `||`)运算符。将这两者结合起来会引发语法错误。

```
const name = *null* || *undefined* ?? 'John Doe'*//Uncaught SyntaxError: Unexpected token '??*
```

如果您需要组合它们，那么您必须用括号将其中一个操作符组括起来。

```
const name = (*null* || *undefined*) ?? 'John Doe'
```

这将消除歧义，任何阅读代码的人都可以立即理解它做什么。编码快乐！

*原载于*[*https://jinoantony.com*](https://jinoantony.com/blog/nullish-coalescing-operator-explained)*。*

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****