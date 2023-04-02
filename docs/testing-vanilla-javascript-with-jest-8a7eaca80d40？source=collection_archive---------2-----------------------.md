# 用 Jest 测试普通 JavaScript

> 原文：<https://javascript.plainenglish.io/testing-vanilla-javascript-with-jest-8a7eaca80d40?source=collection_archive---------2----------------------->

## 两分钟内的完整示例:从安装到测试

![](img/a92258aa41470d9bebf40f21338058a7.png)

Shutterstock

[**Jest**](https://jestjs.io/en/) 是一个注重简单性的 JavaScript 测试框架。它可以和 Angular 这样的框架或者 React 这样的库一起工作。同样，用普通的 JavaScript。

用 Jest 测试普通 JS 非常容易。我将介绍 jest 安装中的一个简单示例，并测试一个简单的函数。

首先，你需要在你的电脑上安装 npm 或者 yarn。

*   安装 NPM:[https://www.npmjs.com/get-npm](https://www.npmjs.com/get-npm)
*   安装纱线:[https://www.npmjs.com/package/yarn](https://www.npmjs.com/package/yarn)

1.  初始化新的 npm 项目

```
**$ npm init***This utility will walk you through creating a* ***package.json*** *file.
It only covers the most common items, and tries to guess sensible defaults.**See `npm help json` for definitive documentation on these fields
and exactly what they do.**Use `npm install <pkg> --save` afterwards to install a package and
save it as a dependency in the package.json file.**Press ^C at any time to quit.
name: (jest_example)**version: (1.0.0) 
description: my first test
entry point: (index.js) 
test command: 
git repository: 
keywords: 
author: 
license: (ISC)**{
  "name": "jest_example",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}**Is this ok? (yes) Aborted.*
```

这将创建一个名为 package.json 的文件

```
*{
  "name": "jest_example",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "****test****": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}*
```

1.  使用 npm 或 yarn 安装 Jest:

```
**$ npm install --save-dev jest
  or
$ yarn add --dev jest***Note: yarn require node version ^8.12.0 || >=9.7.0". Got "8.10.0"*
```

让我们从编写一个简单函数的测试开始，这个函数将两个数相减。

2.创建我们想要测试的函数

```
//subtract.js file.const subtract = (a,b) => a - b;//export default subtract; ES6 , require babel
//Node CommonJS module system:
module.exports = subtract;
```

3.创建测试文件

```
//subtract.test.js file.//import * as myModule from 'subtract.js'; ES6, require babel.
const subtract = require('./subtract');test('subtract 2 - 1 to equal 1', () => {expect(subtract(2, 1)).toBe(1);});
```

4.在 package.json 文件中添加/修改以下内容。

```
{
  "scripts": {
    "test": **"jest"**
  }
}
```

5.最后，运行纱线测试或 npm 运行测试，Jest 将打印以下消息:

```
**$ npm run test**> jest_example@1.0.0 test /jest_example
> jestPASS  ./subtract.test.js
  ✓ subtract 2 - 1 to equal 1 (5ms)Test Suites: 1 passed, 1 total
Tests:       1 passed, 1 total
Snapshots:   0 total
Time:        0.945s, estimated 1s
Ran all test suites.
```

## 结论

这是一个非常基本的例子，但是您可以看到用普通的 JavaScript 用 Jest 创建一个测试是多么容易。

如果这对你有帮助，请点击下面的拍手按钮！非常感谢！