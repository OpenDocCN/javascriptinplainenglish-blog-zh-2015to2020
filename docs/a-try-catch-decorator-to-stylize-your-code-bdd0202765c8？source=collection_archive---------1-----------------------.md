# 一个尝试/捕捉装饰器来风格化你的代码

> 原文：<https://javascript.plainenglish.io/a-try-catch-decorator-to-stylize-your-code-bdd0202765c8?source=collection_archive---------1----------------------->

## 让我们为 ECMA/TypeScript 构建一个 Try/Catch 装饰器来风格化和简化我们的错误处理代码

![](img/1cf517a575c81282c0418a6e7bea4f7c.png)

Photo by [Clément H](https://unsplash.com/@clemhlrdt?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

应用程序有错误。有时这些是我们的错误，比如空引用，或者它们超出了我们的范围，比如来自用户方的错误连接。但是不管它们来自哪里，我们都应该对待它们，这样我们的应用程序才能优雅地继续。

为了处理 ECMA/TypeScript 中的错误，我们用一个`try/catch`块包围我们的代码，或者我们将`.catch()`附加到一个承诺上。这些可以快速生成嵌套代码和样板文件，特别是当我们需要处理许多类型的错误时。

# 装修工

装饰者，顾名思义，装饰我们的代码(函数、类)来添加信息或修改它的行为。装饰器是 JavaScript 的第二阶段提案，是 TypeScript 的一个实验性特性。

为了启用对装饰者的实验性支持，您必须启用您的`tsconfig.json`中的`experimentalDecorators`编译器选项:

```
{
  "compilerOptions": {
    "target": "ES6",
    "experimentalDecorators": true
  }
}
```

一个*装饰器工厂*的基本结构，简单来说就是一个返回表达式的函数，这个表达式将在运行时被装饰器调用，如下所示:

```
const Log = (msg: string): any => {
  return (target: any, propertyKey: string, descriptor: PropertyDescriptor) => {
    // target: class constructor or current object's prototype
    // propertyKey: name of the decorated method
    // descriptor: property descriptor of the method
    modify class or method...
  }
}
```

那么我们将如下使用它:

```
@Log('decorating class')
class Test {
  @Log('decorate method')
  testMethod() {}
}
```

# 履行

现在我们知道了装饰器是什么以及它是如何实现的，让我们编写自己的装饰器。

我们想要编程的是:

*   捕捉特定错误的装饰器
*   捕捉所有错误的装饰器
*   将这些装饰器应用于一个类或一个方法

## 方法装饰者

这里有很多东西需要解开，所以让我们一行一行来。

[1]
我们需要传递给装饰器的处理函数的定义。该函数接收捕获的错误和函数的上下文(`this`)。

【3】
装修工的签名。`Catch` decorator 需要捕捉特定的错误以及处理该错误的函数。

[5–6]
`descriptor.value`保存对原始修饰方法的引用。我们保存此参考供以后使用。

[9]
我们用一个新的函数覆盖了修饰过的方法。

【10，23-25】
我们周围的`try/catch`街区。

[11]
原修饰方法的执行和结果。如果抛出一个错误，它将被前面提到的`try/catch`块捕获。

我们检查修饰的方法是否是异步的(也就是返回一个承诺)。如果是这样，我们返回原始方法的结果，并添加一个`.catch`来处理错误(如果有的话)。

[21–22]
如果修饰的方法是同步的，我们返回它的结果。

[27–28]
我们返回新的描述符，即由`try/catch`包围的原始方法，如果是异步的，则添加`.catch`。

[32]
随着`Catch`装饰器的实现，它只是一个返回装饰器函数的函数，我们可以简单地将`CatchAll`定义为`Catch`，但是已经指定它应该捕捉任何错误。

[34–44]
错误处理函数。我们确认传递的函数确实是一个函数，并且错误属于指定的类型。如果是，我们执行给定的函数。否则，我们再次抛出错误，并转发给下一个装饰器或被装饰的方法。

## 类装饰者

如果我们装饰一个类，我们想要遍历这个类的所有方法并装饰它们。

如果我们修饰一个类，我们可以使用`target`来获得它的所有(非静态)方法。然后我们可以用和以前一样的方法覆盖它们。

[4–7]
如果定义了`descriptor`，那么我们就是在修饰一个方法。我们像以前一样生成一个描述符并返回它。

【8–17】
否则，我们就是在布置一个班级。在这种情况下，`target`是类构造函数，我们可以访问它的原型。我们使用`Reflect.ownKeys(target.prototype)`来获取它所有(非静态)方法的名称，包括构造函数，所以我们需要过滤掉它。
然后我们得到方法的描述符，确认它是一个方法，像以前一样生成新的描述符。我们还需要重新定义修改后的类属性。

[21–50]
`_generateDescriptor`函数做的事情与之前在`Catch`装饰器中做的完全一样。

# 结论

我们现在已经编写了一个装饰器，它用一个`try/catch`块来包围我们的方法，这个块将处理我们定义的错误。

有了这个装饰器，我们可以改变它:

```
class Messenger {
  getMessages() {
    try {
      ...
    } catch(err) {
      if (err instanceof TypeError) {
        console.log(this, err);
      } else {
        throw err;
      }
    }
  } // and the same surrounding block for all the class functions
}
```

收件人:

```
@Catch(TypeError, (err, ctx) => console.log(ctx, err))
class Messenger {
  getMessages() {
    ...
  } // all the other functions
}
```

谢谢你看我的文章！我希望你能从中学到一些新的东西，或者它能帮助你使你的代码更整洁。

如果你不想自己实现装饰器，你可以安装[这个 NPM 包](https://www.npmjs.com/package/@magna_shogun/catch-decorator)。如果你想看到所有的东西，你也可以找到源代码。

# 参考

 [## 装修工#

### 编辑描述

www.typescriptlang.org](https://www.typescriptlang.org/docs/handbook/decorators.html) [](https://www.npmjs.com/package/@magna_shogun/catch-decorator) [## @ magna _ 幕府/catch-decorator

### ECMAScript/TypeScript 捕获类和函数的装饰器。基于@enkot/catch-decorator。npm 安装…

www.npmjs.com](https://www.npmjs.com/package/@magna_shogun/catch-decorator) 

## **简明英语团队的笔记**

你知道我们有四份出版物和一个 YouTube 频道吗？你可以在我们的主页 [**plainenglish.io**](https://plainenglish.io/) 找到所有这些内容——关注我们的出版物并 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达你的爱吧！**