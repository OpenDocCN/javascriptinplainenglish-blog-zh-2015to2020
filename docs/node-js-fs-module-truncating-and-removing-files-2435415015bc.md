# Node.js FS 模块-截断和删除文件

> 原文：<https://javascript.plainenglish.io/node-js-fs-module-truncating-and-removing-files-2435415015bc?source=collection_archive---------6----------------------->

![](img/891e747019c98b8c38d10a547e5b828d.png)

Photo by [Paul Hanaoka](https://unsplash.com/@paul_?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

操作文件和目录是任何程序的基本操作。因为 Node.js 是一个服务器端平台，可以直接与运行它的计算机交互，所以能够操作文件是一个基本特性。幸运的是，Node.js 的库中内置了一个`fs`模块。它有许多功能，可以帮助操纵文件和文件夹。支持的文件和目录操作包括基本的操作，如操作和打开目录中的文件。同样，它也可以对文件做同样的事情。它可以同步和异步地做到这一点。它有一个异步 API，其中包含支持承诺的函数。它还可以显示文件的统计数据。几乎所有我们能想到的文件操作都可以用内置的`fs`模块来完成。在本文中，我们将用`truncate`函数族截断文件，用`unlink`函数族删除文件和符号链接。

# 使用 fs.truncate 系列函数截断文件

我们可以用 Node.js `truncate`系列函数来截断文件。截断文件是将文件缩小到指定的大小。要异步截断文件，我们可以使用`truncate`函数。该函数有 3 个参数。第一个参数是路径对象，可以是字符串、缓冲区对象或 URL 对象。当文件描述符而不是路径被传入第一个参数时，它将自动调用`ftruncate`来截断带有给定文件描述符的文件。传入文件描述符是不赞成的，将来可能会引发错误。第二个参数是文件的长度(以字节为单位),您希望将其截断。默认值为 0。当大小小于原始大小时，任何大于指定大小的额外数据都会丢失。第三个参数是截断操作结束时运行的回调函数。它接受一个`err`参数，当截断操作成功时该参数为`null`，否则该参数包含一个带有错误信息的对象。

要用`truncate`函数截断一个文件，我们可以编写以下代码:

```
const fs = require("fs");
const truncateFile = "./files/truncateFile.txt";fs.truncate(truncateFile, 1, err => {
  if (err) throw err;
  console.log("File truncated");
});
```

如果我们运行上面的代码，您要截断的文件中应该只剩下一个字节的内容。

`truncate`函数的同步版本是`truncateSync`函数。该函数有两个参数。第一个参数是路径对象，可以是字符串、缓冲区对象或 URL 对象。当一个文件描述符而不是路径被传入第一个参数时，它会自动调用`ftruncateSync`来截断带有给定文件描述符的文件。传入文件描述符是不赞成的，将来可能会引发错误。第二个参数是文件的长度(以字节为单位),您希望将其截断。默认值为 0。当大小小于原始大小时，任何大于指定大小的额外数据都会丢失。它返回`undefined`。

我们可以像下面的代码一样使用`truncateSync`函数:

```
const fs = require("fs");
const truncateFile = "./files/truncateFile.txt";try {
  fs.truncateSync(truncateFile, 1);
  console.log("File truncated");
} catch (error) {
  console.error(error);
}
```

如果我们运行上面的代码，您要截断的文件中应该只剩下一个字节的内容。

还有一个`truncate`功能的承诺版本。该函数有两个参数。第一个参数是路径对象，可以是字符串、缓冲区对象或 URL 对象。第二个参数是文件的长度(以字节为单位),您希望将其截断。默认值为 0。当大小小于原始大小时，任何大于指定大小的额外数据都会丢失。当操作成功时，它返回一个没有参数的解析承诺。

要使用 promise 版本的`truncate`函数截断文件，我们可以编写以下代码:

```
const fsPromises = require("fs").promises;
const truncateFile = "./files/truncateFile.txt";(async () => {
  try {
    await fsPromises.truncate(truncateFile, 1);
    console.log("File truncated");
  } catch (error) {
    console.error(error);
  }
})();
```

如果我们运行上面的代码，在您截断的文件中应该还有一个字节的内容。

![](img/1322e0363a992dd4013d32c394d113fd.png)

Photo by [Max Baskakov](https://unsplash.com/@snowboardinec?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 用 fs.unlink 函数族删除文件和符号链接

我们可以用`unlink`函数删除一个文件或一个符号链接。该函数有两个参数。第一个参数是路径对象，可以是字符串、缓冲区对象或 URL 对象。第二个参数是一个回调函数，它接受一个`err`对象，当文件或符号链接删除操作成功时，该对象是`null`，如果操作失败，则包含错误数据。`unlink`函数在任何状态下都不能对目录起作用。要删除目录，我们应该使用`rmdir`函数。

要使用`unlink`函数删除一个文件，我们可以类似下面的代码:

```
const fs = require("fs");
const fileToDelete = "./files/deleteFile.txt";fs.unlink(fileToDelete, err => {
  if (err) {
    throw err;
  }
  console.log("Removal complete!");
});
```

如果我们运行上面的代码，要删除的文件应该会消失。

`unlink`函数的同步版本是`unlinkSync`函数。该函数接受一个参数。唯一的参数是路径对象，它可以是字符串、缓冲区对象或 URL 对象。它返回`undefined`。

我们可以在下面的代码中使用它:

```
const fs = require("fs");
const fileToDelete = "./files/deleteFile.txt";try {
  fs.unlinkSync(fileToDelete);
  console.log("Removal complete!");
} catch (error) {
  console.error(error);
}
```

还有一个`unlink`功能的承诺版。该函数接受一个参数。唯一的参数是路径对象，它可以是字符串、缓冲区对象或 URL 对象。当操作成功时，它返回一个没有任何参数的承诺。

如果我们运行上面的代码，要删除的文件应该会消失。

我们可以在下面的代码中使用它:

```
const fsPromises = require("fs").promises;
const fileToDelete = "./files/deleteFile.txt";(async () => {
  try {
    await fsPromises.unlink(fileToDelete);
    console.log("Removal complete!");
  } catch (error) {
    console.error(error);
  }
})();
```

如果我们运行上面的代码，要删除的文件应该会消失。

当你想连续做多件事，包括调用`unlink`函数时，`unlink`函数的 promise 版本是比`unlinkSync`函数更好的选择，因为它不会占用整个程序，等待文件或符号链接删除操作完成后再继续编程程序的其他部分。

在本文中，我们用`truncate`函数族截断文件，用`unlink`函数族删除文件和符号链接。`truncate`系列函数让我们在截断文件的其余部分时指定要保留的字节数。`unlink`系列函数删除文件和符号链接。如果我们想把这些操作和其他操作一起进行，这些函数的承诺版本。尽管该 API 仍处于试验阶段，但它比这些函数的同步版本好得多，因为它允许顺序和异步操作。此外，它有助于避免我们在太多层次嵌套承诺的回调地狱。