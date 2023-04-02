# JavaScript 测试驱动开发简介

> 原文：<https://javascript.plainenglish.io/test-driven-development-in-javascript-an-intro-26b5a42acd2e?source=collection_archive---------5----------------------->

![](img/e2e9f289a546e43d429d8b1b04bb1c27.png)

Photo by [Ferenc Almasi](https://unsplash.com/@flowforfrank?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

**测试驱动开发(TDD)** 是开发人员社区中广泛使用的术语，但是在你真正理解它为什么有用之前，很难看到它的好处。简单地说，TDD 就是编写测试来定义你的代码应该如何工作，然后编写代码来通过这些测试。

本文将通过编写一个简单的将两个数相加的函数来介绍使用 TDD 过程进行开发的基本方法。

***注:*** *本文期望你对 Jest 有一个基本的了解。*

# 设置

我们将建立一个超级简单的项目来启动和运行，我们不会做任何花哨的东西。

首先创建一个新目录`/tdd-basics`。

## 安装依赖项

让我们开始我们的新项目。我用的是纱线，但是`npm`也可以！

```
yarn init
```

为了运行测试，我们需要 Jest，一个简单的 JavaScript 测试框架。对这个项目不太重要，但 Jest 通常应该是一个开发依赖，因为它只在开发时需要。

```
yarn add --dev jest 
```

## 添加测试脚本

要运行 Jest，我们需要在我们的`package.json`中创建一个脚本

```
{
  ...
  "scripts": {
    "test": "jest"
  },
  ...
}
```

## 创建文件

对于这个练习，我们需要两个文件，一个用于测试，一个用于代码。

```
touch sum.js sum.test.js
```

# 编写测试

在我们编写任何测试之前，我们需要导入 sum 函数以便使用它。

## 首次测试

我们的第一个测试将是检查快乐路径场景，以确保函数做了它应该做的事情。

非常简单，我们只需调用几次函数，并确保输出是我们所期望的。

## 进一步测试

我们不能只写测试来检查快乐路径，我们需要覆盖任何潜在的边缘情况。

我们的 sum 函数应该只接受两个参数，并且这两个参数都应该是整数。我们正在把我们期望函数在这些场景中如何工作的要求放到测试中。

如果我们现在运行测试，你会看到他们都理所当然地失败了。

```
FAIL  ./sum.test.js
  ✕ adds the first number to the second (2 ms)
  ✕ only adds the first two arguments
  ✕ non integer args should throw error (17 ms)
```

# 实现功能

首先，我们需要创建基本函数。

现在我们可以再次运行测试。

```
FAIL  ./sum.test.js
  ✕ adds the first number to the second (2 ms)
  ✕ only adds the first two arguments (1 ms)
  ✕ non integer args should throw error (17 ms)● adds the first number to the secondTypeError: sum is not a function2 | 
      3 | test("adds the first number to the second", () => {
    > 4 |   expect(sum(1, 2)).toBe(3);
        |          ^
      5 |   expect(sum(2, 2)).toBe(4);
      6 |   expect(sum(10, 21)).toBe(31);
      7 | });at Object.<anonymous> (sum.test.js:4:10)
```

还是全部不及格！看来我们忘记导出函数了。

如果我们现在进行测试…

```
FAIL  ./sum.test.js
  ✓ adds the first number to the second (8 ms)
  ✓ only adds the first two arguments (1 ms)
  ✕ non integer args should throw error (7 ms)
```

这样好一点……我们只需要满足最后一个测试用例。

如果参数不是整数，我们需要抛出一个错误。为了简单起见，我们将使用 Lodash。让我们把它作为一个依赖项来添加。

```
yarn add lodash
```

向代码中添加类型检查。

我们只是检查其中一个参数是否不是整数，并抛出一个带有描述性消息的 TypeError。

让我们最后一次进行测试

```
PASS  ./sum.test.js
  ✓ adds the first number to the second (10 ms)
  ✓ only adds the first two arguments (1 ms)
  ✓ non integer args should throw error (10 ms)
```

我们完事了。我们已经编写了一个简单的函数来添加两个满足我们测试的数字。

# 结论

测试驱动开发不仅仅是写测试，它是定义一些代码应该如何工作的需求的一种方式，当你写代码的时候你可以依靠它。这只是一个简单的例子，但是您可以看到在开始编码之前编写测试是如何让您发现错误并更快地开发的。

它还为您提供了快速更改实现的灵活性，而不用担心会破坏某些东西。

TDD 看起来像是额外的开发工作，但是从长远来看，随着回归缺陷的减少、开发时间的减少和没有回溯性的测试编写，它是值得的。