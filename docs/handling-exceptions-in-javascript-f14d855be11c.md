# 在 JavaScript 中处理异常

> 原文：<https://javascript.plainenglish.io/handling-exceptions-in-javascript-f14d855be11c?source=collection_archive---------4----------------------->

![](img/26bc45c81836cf03c8e2dc4b97dfcc00.png)

Photo by [Max Chen](https://unsplash.com/@maxchen2k?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

像任何程序一样，JavaScript 会遇到错误情况，例如，JSON 解析失败，或者变量中意外出现空值。这意味着，如果我们希望我们的应用程序给用户带来良好的用户体验，我们必须优雅地处理这些错误。这意味着我们必须优雅地处理这些错误。错误经常以异常的形式出现，所以我们必须优雅地处理它们。为了处理它们，我们必须使用`try...catch`语句来处理这些错误，这样它们就不会使程序崩溃。

# 试着…接住

要使用`try...catch`块，我们必须使用以下语法:

```
try{
  // code that we run that may raise exceptions
  // one or more lines is required in this block
}
catch (error){
  // handle error here
  // optional if finally block is present
}
finally {
  // optional code that run either 
  // when try or catch block is finished
}
```

例如，我们可以编写以下代码来捕捉异常:

```
try {
  undefined.prop
} catch (error) {
  console.log(error);
}
```

在上面的代码中，我们试图从`undefined`获取一个属性，这显然是不允许的，所以抛出了一个异常。在`catch`块中，我们捕捉到运行`undefined.prop`导致的“类型错误:无法读取未定义的属性‘prop’”并记录异常的输出。所以我们得到的是输出的错误信息，而不是崩溃的程序。

`try...catch`语句有一个`try`块。`try`块内部必须至少有一个语句，并且必须始终使用花括号，这是针对单个语句的事件。那么可以包括`catch`条款或`finally`条款。这意味着我们可以拥有:

```
try {
  ...
} 
catch {
  ...
}try {
  ...
} 
finally{
  ...
}try {
  ...
} 
catch {
  ...
}
finally {
  ...
}
```

`catch`子句有指定当`try`块中抛出异常时该做什么的代码。如果他们的`try`块没有成功并抛出异常，那么`catch`块中的代码将会运行。如果运行了`try`块中的所有代码而没有抛出任何异常，那么`catch`块中的代码将被跳过。

`finally`块在`try`块或`catch`块运行完所有代码后执行。不管是否抛出异常，它总是运行。

`try`块可以相互嵌套。如果内部`try`块没有捕获异常，而外部块有一个`catch`块，那么外部块将捕获内部`try`块抛出的异常。例如，如果我们有:

```
try {
  try {
    undefined.prop
  } finally {
    console.log('Inner finally block runs');
  }
} catch (error) {
  console.log('Outer catch block caught:', error);
}
```

如果我们运行上面的代码，我们应该会看到“内部 finally 块运行”和“外部 catch 块被捕获:类型错误:无法读取未定义的“已记录”的属性“prop ”,这是我们所期望的，因为内部`try`块没有用`catch`块捕获异常，而外部`catch`块捕获了异常。正如我们看到的，内部的 finally 块在外部的 catch 块之前运行。`try...catch...finally`按顺序运行，所以先添加的代码会在后添加的代码之前运行。

到目前为止，我们编写的`catch`块都是无条件的。这意味着它们捕捉任何抛出的异常。`error`对象保存关于抛出的异常的数据。它只保存`catch`块中的数据。如果我们想把数据放在它之外，那么我们必须把它赋给`catch`块之外的一个变量。在`catch`块完成运行后，`error`对象不再可用。

`finally`子句包含在`try`块或`catch`块中的代码执行之后，但在`try...catch...finally`块之下执行的语句之前出现的语句。不管是否抛出异常，它都会被执行。如果抛出异常，那么即使没有`catch`块捕获和处理异常，也会执行`finally`块中的语句。

因此，当错误发生时，`finally`块可以方便地使我们的程序优雅地失败。例如，我们可以放置清理代码，不管是否抛出异常都运行，就像关闭文件读取句柄一样。在运行`try`块中的一行代码时，当抛出异常时，不会执行`try`块中的剩余代码，因此，如果我们被期望关闭`try`中的文件句柄，并且在关闭文件句柄的行运行之前抛出异常，那么为了优雅地结束程序，我们应该在`finally`块中这样做，以确保文件句柄总是被清理。我们可以把不管是否抛出异常都运行的代码像清理代码一样放在`finally`块中，这样我们就不必在`try`和`catch`块中复制它们。例如，我们可以写:

```
openFile();
try {
  // tie up a resource
  writeFile(data);
}
finally {
  closeFile(); 
  // always close the resource
}
```

在上面的代码中，无论`writeFile`运行时是否抛出异常，函数`closeFile`总是运行，从而消除了重复代码。

我们可以嵌套`try`块，如以下代码所示:

```
try {
  try {
    throw new Error('error');
  }
  finally {
    console.log('finally runs');
  }
}
catch (ex) {
  console.error('exception caught', ex.message);
}
```

如果我们查看控制台日志，我们应该看到“最终运行”出现在“异常捕获错误”之前这是因为`try...catch`块中的所有内容都是逐行运行的，即使它是嵌套的。如果我们有更多的嵌套，如下面的代码所示:

```
try {
  try {
    throw new Error('error');
  }
  finally {
    console.log('first finally runs');
  } try {
    throw new Error('error2');
  }
  finally {
    console.log('second finally runs');
  }
}
catch (ex) {
  console.error('exception caught', ex.message);
}
```

我们看到，我们得到了与以前相同的控制台日志输出。这是因为第一个内部`try`块没有捕获异常，所以异常被传播到外部`catch`块并被其捕获。如果我们想运行第二个`try` 块，那么我们必须向第一个`try`块添加一个`catch`块，如下例所示:

```
try {
  try {
    throw new Error('error');
  }
  catch {
    console.log('first catch block runs');
  }  
  finally {
    console.log('first finally runs');
  } try {
    throw new Error('error2');
  }
  finally {
    console.log('second finally runs');
  }
}
catch (ex) {
  console.error('exception caught', ex.message);
}
```

现在我们看到按顺序记录的以下消息:“第一个 catch 块运行”、“第一个最终运行”、“第二个最终运行”、“异常捕获错误 2”。这是因为第一个`try`块有一个`catch`块，所以由`throw new Error('error')`行引起的异常现在被第一个内部`try`块的`catch`块捕获。现在第二个内部`try`模块没有关联的`catch`模块，因此`error2`将被外部`catch`模块捕获。

我们还可以重新抛出在`catch`块中捕获的错误。例如，我们可以编写以下代码来实现这一点:

```
try {
  try {
    throw new Error('error');
  } catch (error) {
    console.error('error', error.message);
    throw error;
  } finally {
    console.log('finally block is run');
  }
} catch (error) {
  console.error('outer catch block caught', error.message);
}
```

正如我们所看到的，如果我们运行上面的代码，那么我们会按顺序记录以下内容:“error error”、“finally block is run”和“outer catch block catched error”。这是因为内部的`catch`块记录了由`throw new Error(‘error’)`抛出的异常，但是在`console.error(‘error’, error.message);`运行之后，我们运行`throw error;`再次抛出异常。然后运行内部`finally`块，然后外部`catch`块捕获重新抛出的异常，该外部`catch`块记录了由内部`catch`块中的`throw error`语句重新抛出的`error`。

由于代码是顺序运行的，我们可以在一个`try`块的末尾运行`return`语句。例如，如果我们想将一个 JSON 字符串解析成一个对象，如果在解析传入的字符串时出现错误，例如，当传入的字符串不是有效的 JSON 字符串时，我们希望返回一个空对象，那么我们可以编写以下代码:

```
const parseJSON = (str) => {
  try {
    return JSON.parse(str);
  }
  catch {
    return {};
  }
}
```

在上面的代码中，我们运行`JSON.parse`来解析字符串，如果它不是有效的 JSON，那么将会抛出一个异常。如果抛出异常，那么将调用`catch`子句返回一个空对象。如果`JSON.parse`成功运行，那么将返回解析后的 JSON 对象。所以如果我们跑:

```
console.log(parseJSON(undefined));
console.log(parseJSON('{"a": 1}'))
```

然后我们在第一行得到一个空对象，在第二行得到`{a: 1}`。

![](img/fe617b60e77fd0377c2132ceb30837a6.png)

Photo by [Sharon McCutcheon](https://unsplash.com/@sharonmccutcheon?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 异步代码中的 Try 块

通过`async`和`await`，我们可以缩短承诺代码。在`async`和`await`之前，我们必须使用`then`函数，我们把回调函数作为所有`then`函数的参数。这使得代码很长，因为我们有很多承诺。相反，我们可以使用`async`和`await`语法来替换`then`及其相关的回调，如下所示。使用`async`和`await`语法来链接承诺，我们也可以使用`try`和`catch`块来捕捉被拒绝的承诺并优雅地处理被拒绝的承诺。例如，如果我们想用一个`catch`块捕捉承诺拒绝，我们可以这样做:

```
(async () => {
  try {
    await new Promise((resolve, reject) => {
      reject('error')
    })
  } catch (error) {
    console.log(error);
  }})();
```

在上面的代码中，因为我们拒绝了在`try`块中定义的承诺，所以`catch`块捕获了承诺拒绝并记录了错误。因此，当我们运行上面的代码时，应该会看到记录的“错误”。尽管它看起来是一个普通的`try...catch`块，但它不是，因为这是一个`async`函数。一个`async`函数只返回承诺，所以我们不能返回除了`try...catch`块中的承诺之外的任何东西。`async`函数中的`catch`块只是链接到 then 函数的`catch`函数的简写。所以上面的代码实际上和:

```
(() => {
  new Promise((resolve, reject) => {
      reject('error')
    })
    .catch(error => console.log(error))
})()
```

我们看到，当上面的`async`函数运行时，我们得到了相同的控制台日志输出。

在`async`功能中`finally`模块也与`try...catch`模块一起工作。例如，我们可以写:

```
(async () => {
  try {
    await new Promise((resolve, reject) => {
      reject('error')
    })
  } catch (error) {
    console.log(error);
  } finally {
    console.log('finally is run');
  }
})();
```

在上面的代码中，因为我们拒绝了在`try`块中定义的承诺，所以`catch`块捕获了承诺拒绝并记录了错误。因此，当我们运行上面的代码时，应该会看到记录的“错误”。运行`finally`块，以便我们得到“最终运行”日志。`async`函数中的`finally`块与将`finally`函数链接到承诺的末尾是一样的，所以上面的代码相当于:

```
(() => {
  new Promise((resolve, reject) => {
      reject('error')
    })
    .catch(error => console.log(error))
    .finally(() => console.log('finally is run'))
})()
```

我们看到，当上面的`async`函数运行时，我们得到了相同的控制台日志输出。

我们上面提到的嵌套`try...catch`的规则仍然适用于`async`函数，因此我们可以编写如下代码:

```
(async () => {
  try {
    await new Promise((resolve, reject) => {
      reject('outer error')
    })
    try {
      await new Promise((resolve, reject) => {
        reject('inner error')
      })
    } catch (error) {
      console.log(error);
    } finally { }
  } catch (error) {
    console.log(error);
  } finally {
    console.log('finally is run');
  }
})();
```

这让我们可以轻松地嵌套承诺，并相应地处理它们的错误。这比我们在拥有`async`函数之前将`then`、`catch`和`finally`函数链接起来要干净。

为了处理 JavaScript 程序中的错误，我们可以使用`try...catch...finally`块来捕捉错误。这可以用同步或异步代码来完成。我们将可能抛出异常的代码放在`try`块中，然后将处理异常的代码放在`catch`块中。在`finally`块中，我们运行任何代码，不管是否抛出异常。`async`函数也可以使用`try...catch`块，但它们和其他`async`函数一样只返回承诺，而普通函数中的`try...catch...finally`块可以返回任何东西。