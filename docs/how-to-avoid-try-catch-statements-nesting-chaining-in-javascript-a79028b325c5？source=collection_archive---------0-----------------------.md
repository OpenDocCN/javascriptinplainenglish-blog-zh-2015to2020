# 如何避免 JavaScript 中的 try/catch 语句嵌套/链接？

> 原文：<https://javascript.plainenglish.io/how-to-avoid-try-catch-statements-nesting-chaining-in-javascript-a79028b325c5?source=collection_archive---------0----------------------->

## 以及如何让你的代码更整洁，看起来更有序！用一种简单、容易、高效的错误处理方法。

![](img/519472cdb7e5c8025ed5ac83505ac4b7.png)

**💡注意:** *经过读者的一些反馈，我想澄清一些事情。首先，下面的模式* ***并不是经典的尝试/捕捉错误处理策略*** *的替代。事实上，在某些用例中，它甚至可以是一个反模式。本文展示这种模式的方式是作为* ***处理错误的一种补充方式*** *当您想要采取直接行动来响应错误时。最后，* ***你将在这里读到的一切都取决于每个人的编程风格*** *还有许多其他可用的模式。谢谢你的好意🙏🏻现在让我们继续文章*

开发人员每天都在编写代码，我们需要用它们来检查潜在的错误。

“Try / Catch”语句无处不在…有时它们甚至是嵌套的或链式的。这导致了这样的例子:

```
async function anyFunction() {
  try {
    const result = await fetch("http://test.com");
  } catch (e) {
    // Some thing
  }try {
    const anotherresult = await someOtherAsync();
  } catch (error) {
    // some other error
  }
}
```

您到处都在编写 try/catch 块，以防止导致应用程序崩溃的错误。有时过度是因为:

*   你想标准化你的错误管理。
*   并且你不想依赖外部库的错误格式(或者定制它)。

你会承认:这真的很无聊、多余而且麻烦。

但是我们将在下面看到，我们可以用少量代码做一些更薄且非常强大的东西。

# 🙏🏻以冷静的方式处理 JavaScript 错误

在每种语言中，处理异常都很常见。

一些语言——比如 PHP——能够在 catch 指令中提供错误类型，并且能够使用多个 catch 块。有些没有，而且处理错误的方式有点糟糕，比如 JavaScript。

您可能会很快陷入这样一种情况，即您正在编写大量带有嵌套或链接的代码，这可能会变得非常冗长且不易于维护。

```
async function anyFunction() {
  try {
    const result = await fetch("http://test.com"); try {
      const another = await fetch("http://blabla.com");
    } catch(anotherError) {
      console.log(anotherError);
    } 
  } catch (e) {
    // Some other error handling
  } try {
    const anotherResult = await someOtherAsync();
  } catch (errorFromAnotherResult) {
    // Some other error
  }
}
```

## **💡一些语言已经有很好的工具来处理错误**

像 GOlang 这样的语言可以从函数的返回语句中返回多个值。这允许我们返回一个 error(或 null)加上一个带有来自 Promise 的期望值的对象，所有的都在同一个语句中。

以下示例是网络订户的 GO 示例。

```
func Listen(host string, port uint16) (net.Listener, error) {
  addr, addrErr := net.ResolveTCPAddr("tcp", fmt.Sprintf("%s:%d", host, port))
  if addrErr != nil {
    return nil, fmt.Errorf("Listen: %s", addrErr)
  }

  listener, listenError := net.ListenTCP("tcp", addr)
  if listenError != nil {
    return nil, fmt.Errorf("Listen: %s", listenError)
  }

  return listener, nil
}
```

正如您所看到的，这与 NodeJS 回调具有“几乎”相同的模式，第一个参数是值，第二个参数是错误(事实上 NodeJS 回调以相反的顺序使用这些参数…但是您得到了逻辑)。

## ✅您也可以使用相同的模式处理 JavaScript 错误

虽然 JS 没有多个 return 语句，但是有另一种方法来实现我们上面看到的模式。

您可以通过返回一个包含两个数据的数组来模仿这种行为，这也适用于任何支持数组的语言。

> ❓:好的，你现在能给我看看代码吗？

为了实现这一点，我们需要用一个小的实用函数来包装我们的异步代码。这个函数将解析和拒绝参数格式化为两个元素的数组。这个包装函数在这里将被称为`to`，但是你可以随意给它取任何名字。

```
/**
 * @param { Promise } promise
 * @param { Object } improved - If you need to enhance the error.
 * @return { Promise }
 */
export function to(promise, improved){
  return promise
    .then((data) => [null, data])
    .catch((err) => {
      if (improved) {
        Object.assign(err, improved);
      }

      return [err]; // which is same as [err, undefined];
    });
}
```

这个函数返回两个元素的数组:

*   **在随后的回调**(如果承诺已解决):如果没有错误，则返回`null`和`data`。
*   **在 catch 回调**(如果承诺被拒绝):由于没有数据，返回可扩展的`err`和作为第二个元素的`undefined`。

我们现在可以使用原始的 try/catch 块，并以这种方式更新它。这里所做的只是用我们的`to`函数包装`someAsyncData`承诺。

```
const [error, result] = await to(someAsyncData());if(error){
  // log something and return ?
}const [error2, result2] = await to(someAsyncData2());if(error2){
  // do something else
} else {
  // Here we are sure that result2 is defined and a valid value
}
```

看起来很酷，不是吗？就这么简单。如果 promise 解析，我们返回一个包含 null 和数据的数组，否则我们返回错误和 undefined。

现在，除了传统的 try/catch 块之外，您还可以使用顺序 like 语法来处理错误。

你知道这种模式吗？你已经在用了吗？请在这里分享你的感受。在我看来，这改变了游戏规则，尽管它很简单，没有什么新意😅。但是请注意，这不是传统处理错误方式的替代，只是尝试一下并建立自己的观点。

这里提供了一些实现这种模式的库:

*   [https://www.npmjs.com/package/await-to-js](https://www.npmjs.com/package/await-to-js)

[**🇫🇷STOP！你是法国人吗🥖？**您也可以访问 ici 以接收法国的私人简讯🙂](https://codingspark.io)