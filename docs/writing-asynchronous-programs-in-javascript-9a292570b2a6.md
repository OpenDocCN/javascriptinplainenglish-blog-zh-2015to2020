# 用 JavaScript 编写异步程序

> 原文：<https://javascript.plainenglish.io/writing-asynchronous-programs-in-javascript-9a292570b2a6?source=collection_archive---------1----------------------->

## 了解如何使用 promises 和 async/await 编写异步程序

![](img/283221382b0962f0ca7ed262ca41bb24.png)

毫无疑问，尽管历史悠久，JavaScript 已经成为当今最流行的编程语言之一。由于 JavaScript 的异步特性，它可能会给初学这门语言的人带来一些挑战。在本文中，我们将使用 promises 和 async/await 编写小型异步程序。通过这些例子，我们将确定一些简单的模式，你可以用在你自己的程序中。

如果你是 JavaScript 新手，在阅读这篇文章之前，你可能想先看看我的另一篇文章。

> 本文中的所有代码示例都是为节点环境编写的。如果您没有安装 Node，您可以查看附录 1 中的说明。即使所有的程序都是为 Node 编写的，您也可以将相同的原则应用于在浏览器中运行的脚本。此外，本文的所有代码示例都可以在 [Gitlab](https://gitlab.com/aj_meyghani/manage-async) 上获得。

# 介绍

不管人们是否相信 JavaScript 是一种真正的编程语言，事实是它不会很快走向任何地方。如果你是一名网站开发者，你不妨花些时间了解它的好与坏。

JavaScript 是单线程的，支持非阻塞异步流。如果你是这门语言的新手，当事情没有按照你期望的方式运行时，它会变得非常令人沮丧。异步编程需要更多的耐心和不同于同步编程的不同思维。

在同步模式中，每件事都按顺序发生，一次一件。正因为如此，对程序进行推理变得更加容易。但是在异步模型中，操作可以在任何时间点以任何顺序开始或结束。因此，仅仅依靠一个序列是不够的。异步编程需要在程序流和设计方面进行更多的思考。

在本文中，我们将探索几个简短的异步程序。我们将从简单的程序开始，逐步发展到更复杂的程序。下面是我们将要编写的脚本的概述:

*   将一个文件的内容写入另一个文件的脚本。
*   将多个文件的内容写入新文件的脚本。
*   一个脚本，用于解析和格式化目录中的 CSV 文件，并将新的 CSV 文件输出到另一个文件夹。

# 承诺和异步/等待

让我们花点时间快速回顾一下承诺和异步/等待的基础知识。

**承诺**

*   承诺是表示异步操作结果的对象。
*   承诺要么以“成功”值解决，要么以“失败”值拒绝。
*   一般来说，解析的值是通过回调到`then`块的参数来访问的。被拒绝的值可以通过回调的参数访问到一个`catch`块。
*   在现代 JavaScript 环境中，您可以通过全局对象访问 promise 构造函数，如`Promise`。
*   可以使用`Promise`构造函数和`new`关键字创建一个承诺。那就是:
*   `const p = new Promise((r, j) => {});`
*   `r`回调用于解析带有值的承诺，而`j`回调用于拒绝承诺。
*   `Promise`构造函数有一些有用的静态方法，比如`all`、`race`、`resolve`和`reject`。`all`方法接受一个承诺数组，并将尝试同时解析所有承诺，并返回一个解析为包含已解析值的数组的承诺。`race`方法接受一个承诺数组，并解析或拒绝第一个完成的承诺。`resolve`方法创建一个承诺，并将其解析为给定值。`reject`方法创建一个承诺，并用给定的值拒绝它。

**异步/等待**

*   async/await 函数的目的是简化同步使用承诺的行为，并对一组承诺执行一些行为。[来自 MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function)
*   正如承诺类似于结构化回调，async/await 类似于组合生成器和承诺。[来自 MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function)
*   可以用`async`关键字将函数标记为异步函数。也就是:`async function hello() {}`或者`const hello = async() => {};`。
*   一个`async`函数总是返回一个承诺。如果从`async`函数返回一个值，它将被隐含地包装在一个承诺中。
*   如果在一个`async`函数中抛出了一个未被捕获的异常，那么返回的承诺将被异常拒绝。
*   在返回承诺的语句之前，可以在`async`函数中使用`await`运算符。在这种情况下，函数的执行会“暂停”,直到承诺被解决或拒绝。
*   `await`操作符只在`async`函数中有效。

# 读写单个文件

在本节中，我们将编写一个脚本来读取单个文件的内容，并将结果写入一个新文件。

> 您可以在 [Gitlab](https://gitlab.com/aj_meyghani/manage-async/tree/master/read-write-single-file) 上访问该部分的所有脚本。

首先，我们将为程序的入口点创建一个`async`函数:

```
async function main() {
  // body goes here...
}
```

然后，我们需要创建两个承诺，一个代表文件的内容。另一个表示将内容写入另一个文件的结果:

```
async function main() {
  const fileContent = readFile("./file.txt", "utf-8");
  const writeResult = writeFile("./file-copy.txt", fileContent);
}
```

在上面的代码片段中，`readFile`和`writeFile`都是异步的，它们都返回一个承诺。因此，首先我们需要确保我们`await`了`readFile`的结果，这样我们就可以在`writeFile`中使用它:

```
async function main() {
  const fileContent = **await** readFile("./file.txt", "utf-8");
  const writeResult = writeFile("./file-copy.txt", fileContent);
}
```

最后，我们可以决定从`main`函数返回什么。这里我们将返回我们正在写入的新文件的名称。请注意，返回值将自动包装在承诺中。但是我们需要确保在点击函数的最后一行之前，对`writeFile`的结果进行`await`:

```
async function main() {
  const fileContent = **await** readFile("./file.txt", "utf-8");
  const writeResult = **await** writeFile(
  "./file-copy.txt", fileContent);
  return "file-copy.txt";
}
```

现在，我们可以调用 main 函数，并将结果或任何未捕获的异常记录到控制台:

```
main()
.then(r => console.log("Result:", r))
.catch(err => console.log("An error occurred", err));
```

为了使程序完整，我们需要要求`fs`模块并保证`fs.readFile`和`fs.writeFile`。完整的脚本如下所示:

```
const util = require("util");
const fs = require("fs");
const readFile = util.promisify(fs.readFile);
const writeFile = util.promisify(fs.writeFile);

async function main() {
  const **fileContent** = **await readFile(**"./file.txt", "utf-8");
  const writeResult = **await writeFile**(
  "./file-copy.txt", **fileContent**);
  return "file-copy.txt";
}

main()
.then(r => console.log("Result:", r))
.catch(err => console.log("An error occurred", err));
```

在上面的片段中，我们承诺了`fs.writeFile`和`fs.readFile`。Promisify 可以将任何回调样式的函数转变为基于承诺的函数，只要它们遵循节点约定。

现在我们来谈谈错误处理。有几种方法可以处理错误，这完全取决于你需要多少控制。例如，在上面的代码片段中，我们基本上捕捉到了任何可能发生在`catch`块中的`main`函数中的错误。这是因为在一个`async`函数中，任何未被捕获的异常都会立即导致函数返回一个被异常拒绝的承诺。

但是，假设您需要更多的控制，并且希望根据每个异步操作可能出现的错误做不同的事情。在这种情况下，您可以在每个异步操作上使用 try-catch 块或者使用`catch`块。首先，让我们看看如何使用 try-catch 块。

```
async function main() {
  let fileContent;
  **try {**
    fileContent = **await readFile**("./files.txt", "utf-8");
  **} catch(err) {**
    **return** {message: "Error while reading the file", error: err};
  **}**

  **try {**
    const writeResult = **await writeFile**(
    "./file-copy.txt", fileContent);
  **} catch(err) {**
    **return** {message: "Error while writing the file", error: err};
  **}**

  return "file-copy.txt";
}
```

在上面的代码片段中，我们添加了两个 try-catch 块。此外，我们在第一个块的外部创建了`fileContent`变量，这样它在整个`main`函数中都是可见的。请注意，在每个 try-catch 块中，如果有错误，我们将返回一个对象。错误对象包含一个消息字段和错误的详细信息。现在，如果发生任何错误，该函数将立即返回我们的自定义错误对象。请记住，返回的对象将自动包装在承诺中。我们可以像以前一样调用`main`函数，但是这次我们可以在`then`块中检查错误对象:

```
main()
.**then(r => {**
  **if(r.error) {**
    return console.log(
    "An error occurred, recover here. Details:", r);
  }
  return console.log("Done, no error. Result:", r);
})
.**catch(err** => console.log("An error occurred", err));
```

注意，在`then`块中，我们正在检查被解析的对象是否有错误。如果有，我们就在那里处理。否则，我们只需将结果记录到控制台。另一个 catch 块将捕获任何运行时错误或任何其他程序没有处理的错误。

除了 try-catch 块，我们可以使用与每个承诺相关联的`catch`块:

```
async function main() {
  const fileContent = **await readFile**("./file.txt", "utf-8")
  **.catch(err => ({**
    message: "Error while reading the file", error: err,
  }));

  if (fileContent.error) {
    return fileContent;
  }

  const writeResult = **await writeFile(**
  "./file-copy.txt", fileContent)
  .then(result => ({}))
  **.catch(err => ({**
    message: "Error while writing the file", error: err,
  }));

  if(writeResult.error) {
    return writeResult;
  }

  return "file-copy.txt";
}
```

如果你注意到，我们为每个承诺调用了`catch`方法，并且我们返回了一个定制的错误对象，类似于前面的例子。如果任何一步有错误，我们将简单地返回结果，其中只包含我们的自定义错误对象。

然而，对于第二个操作，如果写操作成功，我们将显式返回一个空对象。这是因为如果操作成功，`writeFile`将解析为`undefined`。因此，我们将无法访问`undefined`值的`error`字段。这就是为什么我们显式地返回一个承诺，如果写操作成功，它将解析为一个空对象。

我们还可以有选择地创建两个助手函数，以避免我们编写相同的样板代码:

```
const call = (promise) =>
  promise.then(r => r == null ? ({result: r}): r)
  .catch(error => ({error}));

const error = (result, msg) => ({error: result.error, message: msg});
```

`call`函数接受一个承诺并返回一个承诺，该承诺要么用一个空对象解析(如果结果为 null 或未定义),要么简单地解析为操作的结果。如果有错误，promise 将解析为一个对象，该对象包含一个包含错误值的错误字段。

`error` helper 函数接受一个结果和一条消息，它将返回一个包含结果错误和自定义可选消息的对象。添加了两个助手函数后，我们可以更新我们的`main`函数:

```
async function main() {
  const fileContent = **await call**(readFile("./file.txt", "utf-8"));
  if(fileContent.error) {
    return error(fileContent, "Error while reading the file");
  }

  const writeResult = **await call(**
  writeFile("./file-copy.txt",  fileContent));

  if(writeResult.error) {
    return error(writeResult, "Error while writing the file");
  }

  return "file-copy.txt";
}
```

如您所见，我们将每个操作传递给了`call`函数。然后我们检查是否有错误。如果是这样，那么我们只需调用我们的`error`函数来返回一个带有自定义错误消息的自定义错误。完整的代码片段如下所示:

```
const util = require("util");
const fs = require("fs");
const readFile = util.promisify(fs.readFile);
const writeFile = util.promisify(fs.writeFile);

const call = (promise) =>
  promise.then(r => r == null ? ({result: r}): r)
  .catch(error => ({error}));

const error = (result, msg) => ({
error: result.error, message: msg});

async function main() {
  const fileContent = **await call(**readFile("./file.txt", "utf-8"));
  if(fileContent.error) {
    return error(fileContent, "Error while reading the file");
  }

  const writeResult = **await call(**
  writeFile("./file-copy.txt", fileContent));

  if(writeResult.error) {
    return error(writeResult, "Error while writing the file");
  }

  return "file-copy.txt";
}

main()
.then(r => {
  if(r.error) {
    return console.log(
    "An error occurred, recover here. Details:", r);
  }
  return console.log("Done, no error. Result:", r);
})
.catch(err => console.log("An error occurred", err));
```

为了减少更多的样板文件，使事情更加模块化，我们可以做两件事:

*   我们可以使用`fs-extra`并删除所有对`util.promisify`的调用。
*   我们还可以将这两个助手函数移动到它们自己的文件中。

之后，我们将有以下内容:

```
const fs = require("fs-extra");
const {error, call} = require("../call");

async function main() {
  const fileContent = **await call(**
  fs.readFile("./file.txt", "utf-8")); if(fileContent.error) {
    **return error**(fileContent, "Error while reading the file");
  }

  const writeResult = **await call(**
  fs.writeFile("./file-copy.txt", fileContent));

  if(writeResult.error) {
    **return error**(writeResult, "Error while writing the file");
  }

  return "file-copy.txt";
}

main()
.then(r => {
  if(r.error) {
    return console.log(
    "An error occurred, recover here. Details:", r);
  }
  return console.log("Done, no error. Result:", r);
})
.catch(err => console.log("An error occurred", err));
```

请注意，由于我们使用的是`fs-extra`，如果我们不向方法传递回调，函数将默认返回一个承诺。这就是为什么我们删除了所有的 promisify 调用，并将所有的`fs`调用直接转换到`fs`变量上。此外，我们将两个助手函数移到了它们自己的名为`call.js`的文件中。

# 读写多个文件

在本节中，我们将编写一个脚本来读取多个文件的内容，并将结果写入新文件。

> 您可以在 [Gitlab](https://gitlab.com/aj_meyghani/manage-async/tree/master/read-write-multiple) 上访问该部分的所有脚本。

本例的设置与前一个非常相似:

```
const fs = require("fs-extra");

async function main() {
  const files = ["files/file1.txt", "files/file2.txt"];
  // ...
}

main()
.then(console.log)
.catch(err => console.log("An error occurred", err));
```

在上面的代码片段中，首先我们需要拥有所有基于承诺版本的`fs`方法的`fs-extra`模块。然后，我们定义主`async`函数作为程序的入口点。我们还定义了一个数组，其中包含我们将要读取的文件的硬编码路径。

接下来，我们将编写一个 for 循环，遍历文件路径并读取每个文件的内容:

```
const fs = require("fs-extra");

async function main() {
  const files = ["files/file1.txt", "files/file2.txt"];

  **for (const file of files) {** // A
    const content = **await** fs.readFile(file, "utf-8"); // B
    console.log(content); // C
  **}**
}

main()
.then(console.log)
.catch(err => console.log("An error occurred", err));
```

在 A 行我们定义了 for 循环。在 B 行上，我们对`fs.readFile`的结果进行`await`，并将其赋给`content`变量。最后，在 C 行，我们将内容记录到控制台。让我们用一个实际的写入文件操作来代替 log 语句:

```
const fs = require("fs-extra");

async function main() {
  const files = ["files/file1.txt", "files/file2.txt"];

  **for (const file of files) {**
    const content = **await** fs.readFile(file, "utf-8");
    const path = file.replace(".txt", "-copy.txt"); // A
    const writeResult = **await** fs.writeFile(path, content); // B
  **}**

  return files; // C
}

main()
.then(console.log)
.catch(err => console.log("An error occurred", err));
```

在上面的代码片段中，首先我们在 a 行定义了文件的路径。然后，在 B 行，我们将结果写入新的路径，并确保在它上面也有`await`。我们需要在这里`await`，因为我们想确保在我们移动到下一个文件之前写操作已经完成。最后在 C 行，我们返回输入文件路径。

现在，上面的实现还可以，但是我们可以做得更好。在上面的实现中，我们一次处理一个文件。也就是说，我们等待每个文件的读写操作完成，然后再移动到下一个文件。我们实际上可以通过创建一个承诺数组来并发地运行每个读写进程，其中每个承诺代表一个文件上的读写操作。最后，我们可以使用`Promise.all`同时处理所有的承诺:

```
const fs = require("fs-extra");

async function main() {
  const files = ["files/file1.txt", "files/file2.txt"];

  const **readWrites** = []; // A

  for (const file of files) { // B
    **readWrites.push((async() => {** // C
      const content = **await** fs.readFile(file, "utf-8"); // D
      const path = file.replace(".txt", "-copy.txt"); // E
      return **await** fs.writeFile(path, content); // F
    **})());**
  }

  return **await** Promise.all(**readWrites**); // G
}

main()
.then(console.log)
.catch(err => console.log("An error occurred", err));
```

在上面的代码片段中，我们在 A 行定义了一个数组来保存读写承诺。在 B 行，我们开始遍历每个文件路径的 for 循环。在第 C 行，我们将一个自调用的`async`函数推送到`readWrites`数组。在每个`async`函数体内，我们读取每个文件的内容并写入一个新文件。在 F 行，我们返回`fs.writeFile`的结果，它是一个承诺对象。最后，在 G 行，我们使用`Promise.all`来同时处理所有的承诺。我们还对结果进行了`await`，它解析为保存写结果的单个数组。如果写操作成功，我们应该得到一个未定义值的数组。这是因为如果没有错误发生，写方法解析为`undefined`。

即使上面的实现完成了工作，我们还可以做得更好一点。我们可以用一个`async`函数在`files`数组上使用 map 方法，并消除对自调用`async`函数的需要。这也更容易理解:

```
const fs = require("fs-extra");

async function main() {
  const files = ["files/file1.txt", "files/file2.txt"];

  const readWrites = **files.map(async file => {** // A
    const content = **await fs.readFile**(file, "utf-8"); // B
    return **await fs.writeFile**(
    file.replace(".txt", "-copy.txt"), content); // C
  });

  return **await Promise.all**(readWrites); // D
}

main()
.then(console.log)
.catch(err => console.log("An error occurred", err));
```

在上面的代码片段中，在第 A 行我们调用了`files`数组上的`map`,并给它传递了一个`async`函数。在`async`函数中，我们简单地执行读写操作。最后在 D 行，我们调用`Promise.all`并传递`readWrites`数组。`readWrites`数组保存承诺，其中每个承诺代表每次读写的结果。

现在，让我们扩展上面的例子。让我们创建一个文件夹，把所有的新文件放进去。我们将需要创建一个`async`函数，它在我们进入读写操作之前为我们创建输出文件夹:

```
async function prepare() {
  **await** **fs.remove**("output"); // A
  return **await fs.mkdir**("output"); // B
}
```

在上面的代码片段中，首先我们创建了一个名为`prepare`的`async`函数。在第 A 行，首先我们删除`output`文件夹，如果它已经存在的话。我们也等待承诺得到解决，然后再移到 B 行。在 B 行，我们创建了`output`文件夹，我们也等待它完成。现在，在开始读写操作之前，我们可以在`main`函数中使用`prepare`函数:

```
const fs = require("fs-extra");

const files = ["files/file1.txt", "files/file2.txt"];
const output = "output";

async function prepare() {
  await fs.remove(output);
  return await fs.mkdir(output);
}

async function main() {
  **await prepare();** // A

  const readWrites = files.map(async file => {
    const content = await fs.readFile(file, "utf-8");
    const path = file.replace("files", output); // B
    return await fs.writeFile(path, content);
  });

  return await Promise.all(readWrites);
}

main()
.then(console.log)
.catch(err => console.log("An error occurred", err));
```

在第 A 行，我们等待`prepare`函数完成，然后继续读写操作。我们还更新了 b 行的输出文件路径。脚本的其余部分基本相同。我们还将`files`和`output`变量移出了主函数。如果您运行上面的脚本，您应该会看到一个包含每个输入文件副本的`output`文件夹。

# 格式化 CSV 文件

在本节中，我们将编写一个脚本来读取几个 CSV 文件，格式化它们，并将结果写入另一个目录。

> 您可以在 [Gitlab](https://gitlab.com/aj_meyghani/manage-async/tree/master/format-csv) 上访问该部分的所有脚本。

下面是脚本需要执行的每个任务的概述:

*   通过检查扩展名和文件统计信息来识别目录中的 CSV 文件(一级深度)。
*   读取每个文件的内容，并使用 CSV 解析器解析内容。
*   对每个文件执行基本格式化。
*   Stringify 格式的结果，并将每个结果写入一个新的 CSV 文件，并将它们放在输出文件夹中。

使用上述任务，我们可以定义以下步骤:

*   如果输出文件夹不存在，首先创建一个输出文件夹。换句话说，无论如何都要删除输出目录，然后重新创建它。
*   然后，识别给定目录中的 CSV 文件。
*   然后，读取每个文件的内容，进行格式化，并将结果写入输出目录中的一个新文件。

使用上面的流程，我们可以为每个步骤定义以下函数:

```
async function setup() {}

async function csvFiles(inputFolder) {}

function format(content) {}

async function formatWrite() {}

async function main() {
  const src = "input-folder";
  const [output, files] = await Promise.all([ // A
    setup(), csvFiles(src),
  ]);

  return await Promise.all( // B
    files.map(async file => formatWrite(file, output))
  );
}
```

*   `setup`函数将处理输出目录的创建。它将返回一个解析为输出目录名称的承诺。
*   `csvFiles`函数将查找给定输入文件夹中的所有 CSV 文件，并返回一个文件路径数组。
*   `format`函数将对解析后的 CSV 文件的内容进行一些基本的格式化。
*   `main`功能是程序的入口点。首先，我们将同时运行`setup`和`csvFiles`。但是我们会等他们两个都结束。之后，我们将创建一个承诺数组，每个承诺代表读取、格式化和写入每个文件。我们等待它结束，然后返回结果。

先说`setup`函数。`setup`功能做的不多。它会删除输出文件夹，无论它是否存在。然后，它创建该文件夹，并返回该文件夹的名称:

```
async function setup() {
  const output = "output";
  **await fs.remove**(output);
  **await fs.mkdir**(output);
  return output;
}
```

接下来，我们来看看`csvFiles`函数。这个函数基本上是读取一个文件夹的内容，然后找出哪些是文件，然后过滤掉扩展名为`.csv`的文件。它将只执行一个级别的深度:

```
async function csvFiles(inputFolder) {
  const dirContent = **await fs.readdir**(inputFolder); // A
  const paths = dirContent.map(c => path.join(inputFolder, c)); // B

  return **await** Promise.all(**paths.map(async p => {** // C
    const isFileAndCSV = 
    ((**await fs.stat(p)**).isFile() && /\.csv$/.test(p)); // D
    return isFileAndCSV ? p : "";
  }))
  .then(paths => paths.filter(v => v)); // E
}
```

*   在第 A 行，我们调用`fs.readdir`来读取输入文件夹的内容。然后我们等待结果返回，并将结果存储在`dirContent`变量中。
*   在第 B 行，我们创建了一个完整路径的数组，从输入文件夹开始，以及上一步的结果。
*   在 C 行，我们开始识别 CSV 文件。
*   在第 D 行，首先我们调用`fs.stat`来获取路径的状态。然后我们等待结果，并使用`isFile`方法来识别给定的路径是否是一个文件。我们还使用正则表达式来检查给定文件的扩展名。
*   一行 E，我们过滤掉所有不为假的值。最后，结果是一个承诺数组，其中每个承诺都解析为一个 CSV 文件。我们使用`Promise.all`来同时履行承诺，并返回已解析的文件数组。

我们要看的下一个函数是`formatWrite`函数。这个函数主要是读取每个文件的内容，用 CSV 解析器对其进行解析，然后将格式化后的内容写入一个新文件:

```
async function formatWrite(file, output) {
  const content = **await fs.readFile**(file, "utf-8"); // A
  const parsed = **await csvParse**(content); // B
  const formatted = format(parsed); // C
  const stringified = **await csvStringify**(formatted); // D
  const outPath = path.join(
  output, file.split("/").slice(-1)[0]); // E
  **await fs.writeFile**(outPath, stringified); // F
  return file;
}
```

*   第一行 A，我们读取给定文件的内容，等待结果，并将其存储在`content`变量中。
*   第 B 行，我们解析上一步的结果并等待它完成。然后，我们将结果存储在`parsed`变量中。
*   第 C 行，我们对上一步的结果调用一个简单的`format`函数，并将它存储在`formatted`变量中。
*   一行 D，我们对格式化的内容调用`csvStringify`函数，然后等待它完成。然后，我们将结果存储在`stringified`变量中。
*   一行 E，我们通过将输出路径与文件名连接起来来定义输出文件的路径。
*   第 F 行，我们把字符串化的内容写到上一行的路径中，然后等待它完成。
*   最后，我们返回我们处理过的输入文件的名称。

真的是这样。以下是完整的程序，包括所有要求的语句和`format`功能的实现:

```
const fs = require("fs-extra");
const util = require("util");
const path = require("path");
const csvParse = util.promisify(require("csv-parse"));
const csvStringify = util.promisify(require("csv-stringify"));

async function setup() {
  const output = "output";
  await fs.remove(output);
  await fs.mkdir(output);
  return output;
}

async function csvFiles(inputFolder) {
  const dirContent = await fs.readdir(inputFolder);
  const paths = dirContent.map(c => path.join(inputFolder, c));

  return await Promise.all(paths.map(async p => {
    const isFileAndCSV = 
    ((await fs.stat(p)).isFile() && /\.csv$/.test(p));
    return isFileAndCSV ? p : "";
  }))
  .then(paths => paths.filter(v => v));
}

function format(content) {
  return content.map((v, i) => {
    if(i === 0) {
      return v.map(h => h.toUpperCase());
    }
    return v;
  });
}

async function formatWrite (file, output) {
  const content = await fs.readFile(file, "utf-8");
  const parsed = await csvParse(content);
  const formatted = format(parsed);
  const stringified = await csvStringify(formatted);
  const outPath = path.join(output, file.split("/").slice(-1)[0]);
  await fs.writeFile(outPath, stringified);
  return file;
}

async function main() {
  const src = "input-folder";
  const [output, files] = await Promise.all([
    setup(), csvFiles(src),
  ]);

  return await Promise.all(
    files.map(async file => formatWrite(file, output))
  );
}

main()
.then(console.log)
.catch(console.log);
```

如果我们需要并发处理大量文件，我们可以限制一次处理的文件数量。为此，我们可以使用类似于`p-limit`的模块。在我们需要它之后，我们可以更新 main 函数，将并发任务限制为一次两个承诺:

```
async function main() {
  const src = "input-folder";
  const [output, files] = await Promise.all([
    setup(), csvFiles(src),
  ]);

  /* limit concurrent tasks to 2 */
 **const limit = pLimit(2); // A**  return await Promise.all(
    files.map(file => **limit(() => formatWrite(file, output))**) // B
  );
}
```

在第 A 行，我们从`pLimit`创建了一个限制函数，并指定我们希望一次运行多少个并发任务。在第 B 行，我们用`limit`包装了我们的`formatWrite`函数，它会处理剩下的事情。你可以在 [Gitlab](https://gitlab.com/aj_meyghani/manage-async/blob/master/format-csv/main2.js) 上看到完整的脚本。

# 结论

JavaScript 无疑已经取得了长足的进步，并且随着 async/await 的出现，编写更好的异步程序变得更加容易。既然我们已经到了文章的结尾，让我们回顾一些重要的要点:

*   我们可以将异步任务分为并发流和顺序流。我们可以捕获承诺中的流程，并决定程序的哪些部分应该并发运行，哪些部分应该顺序运行。
*   我们可以使用数组的`Promise.all`和`map`方法来创建承诺并同时处理它们。我们还可以在`Promise.all`之前使用`await`操作符来等待所有承诺被解析。那就是:
*   `await Promise.all(inputs.map(async v => {}));`
*   如果我们想在`async`函数中使用 try-catch 块，我们需要在任何承诺值或返回承诺的函数之前使用`await`操作符。
*   如果系统资源是一个问题，或者如果我们正在处理大量输入，限制并发任务通常是一个好主意。我们可以使用像`[p-limit](https://github.com/sindresorhus/p-limit)`这样的包来限制同时运行的任务数量。

# 附录 1:安装节点

安装 Node 最简单和最一致的方式是通过版本管理器，如 [NVM](https://github.com/creationix/nvm) 。首先，使用以下内容安装 NVM:

```
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash
```

然后检查您的“profile”文件，查看是否添加了以下条目:

```
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

然后重启您的终端，确保您可以获得`nvm --version`的输出。之后，只需运行`nvm install 8`来安装最新的节点 8。之后，运行`node -v`和`npm -v`来验证节点和 Npm 都可用。