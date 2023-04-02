# React 中的测试，第 3 部分:Jest & Jest-Dom

> 原文：<https://javascript.plainenglish.io/testing-in-react-part-3-jest-jest-dom-7a8a03ae60b?source=collection_archive---------1----------------------->

![](img/f36da603f8dc690412d4833e99231170.png)

Photo by [Stephane Coudassot-Berducou](https://unsplash.com/@hi_i_am_steph?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

> 本文是 React 中测试系列的一部分:
> 
> [React 中的测试，第 1 部分:类型&工具](https://medium.com/javascript-in-plain-english/testing-in-react-part-1-types-tools-244107abf0c6)
> 
> [React 中的测试，第 2 部分:React 测试库](https://medium.com/javascript-in-plain-english/testing-in-react-part-2-react-testing-library-f32432b93c6c)
> 
> **React 的测试，第 3 部分:Jest & Jest-Dom**
> 
> [React 中的测试，第 4 部分:酶](https://medium.com/javascript-in-plain-english/testing-in-react-part-4-enzyme-9b030ad616ae)
> 
> [React 中的测试，第 5 部分:使用 Cypress 的端到端测试](https://medium.com/@bryn.bennett/testing-in-react-part-5-end-to-end-testing-with-cypress-bd2bf8d3385f)
> 
> [React 中的测试，第 6 部分:React 测试库、Jest、Enzyme 和 Cypress 的真实测试](https://medium.com/javascript-in-plain-english/testing-in-react-part-6-real-world-testing-with-react-testing-library-jest-enzyme-and-cypress-9c73436d95d8)

到目前为止，在本系列中，我们已经介绍了 React 的各种类型的测试和工具，以及最流行的测试库之一， [React 测试库](https://medium.com/javascript-in-plain-english/testing-in-react-part-2-react-testing-library-f32432b93c6c)。如果你还没有读过这些，我建议你读一读。Jest 与 React 测试库一起使用，为了理解通过渲染组件树进行测试，理解这两者是很重要的。

# 快速回顾一下

用 Jest 和 React 测试库测试渲染组件树完全是为了测试用户体验。它是从用户的角度测试应用程序的行为，而不是测试代码的实现。

为了做到这一点，您必须有两个基本部分——在其上执行测试的呈现元素(即组件),以及要测试的 DOM 节点的特定质量或属性(即组件中按钮的存在)。

考虑 React 测试库和 Jest 的一个简单方法是，React 测试库处理元素的呈现，Jest 处理 DOM 节点的测试。

# 笑话世界

现在*在技术上，* React 测试库也通过一个叫做 [Jest-Dom](https://testing-library.com/docs/ecosystem-jest-dom) 的东西提供了一些测试功能。文档将 Jest-Dom 描述为:

> `React Testing Library`的配套库，为 Jest 提供定制的 DOM 元素匹配器。

简单地说，匹配器是测试的元素，表示类似`.toEqual()`或`.toBeVisible`的东西。我们将简要介绍 Jest-Dom，但实际上它只是一个扩展列表，提供了更多不同属性的选项供测试。重要的是要知道，它会在你需要的时候出现，如果你在 Jest 的匹配器里找不到你想要的，Jest-Dom 可能就有你想要的。

# 玩笑

我认为 Jest 是创建测试环境的工具。它提供了呈现用于测试的组件的上下文，并创建了测试套件和测试块。

Jest 可以在您的`package.json`文件、`jest.config.js`文件或`—-config <path/to/file.js|cjs|mjs|json>`选项中配置。您可以在这里了解更多关于配置[。](https://jestjs.io/docs/en/next/configuration)

## 测试套件

测试套件将相关测试分组在一起。它们是使用`describe` 功能创建的。`describe`接受字符串名称/描述和函数作为参数:`describe(name, fn)`。

## 测试用例

测试用例是由测试套件分组的实际测试。它们是使用`test`方法编写的，该方法接受名称、函数和可选超时:`test(name, fn, timeout)`。

例如，您可以写道:

```
const isTruthy = true;describe('isTruthy variable', () => {
  test('is true', () => {
    expect(isTruthy).toBe(true);
  });

  test('is not false', () => {
    expect(isTruthy).not.toBe(false);
  })
})
```

# 火柴人

上面的`.toBe()`叫做匹配器——这就是我们前面提到的东西，其中的 React Testing Library 也通过 Jest-Dom 提供了一些。可用的匹配器有很长的列表，从验证真与假(如`.toBeNull()`和`.toBeDefined()`)到验证数字量(如`.toBeGreaterThan()`和`.toBeCloseTo()`),到用于字符串匹配的`.toMatch()`、用于数组的`.toContain()`和用于错误处理的`.toThrow()`。

您可以在这里找到 Jest [提供的媒人完整列表，在这里](https://jestjs.io/docs/en/expect)找到 Jest-Dom [提供的媒人完整列表。](https://github.com/testing-library/jest-dom#custom-matchers)

# 异步测试

像 React 测试库一样，Jest 为 JavaScript 中异步运行的代码提供测试。具体来说，您可以测试回调、承诺和异步/等待。

## 复试

可以通过简单地将`done`参数传递给测试函数来测试回调，而不是传递一个空参数。使用上面的例子，它看起来像这样:

```
const isTruthy = true;describe('isTruthy variable', done => {
  test('is true', () => {
    expect(isTruthy).toBe(true);
  });

  test('is not false', done => {
    expect(isTruthy).not.toBe(false);
  })
})
```

现在这是没有用的，因为我们在这里没有使用`done`，但这就是它看起来的样子。我将借用文档中的例子，因为没有更好的方法来解释它。下面是一个例子，其中`fetchData()` 调用一个回调函数`callback()`，期望数据等于“花生酱”。你的第一反应可能是这样测试它:

```
test('the data is peanut butter', () => {
  **function** **callback**(data) {
    expect(data).toBe('peanut butter');
  } fetchData(callback);
});
```

但是这个不行。这就是你可以使用`done()`回调的地方。Jest 知道要等到`done()`完成后才能完成测试。

```
test('the data is peanut butter', done => {
  **function** **callback**(data) {     
    **try** {       
      expect(data).toBe('peanut butter');       
      done();     
    } **catch** (error) {       
      done(error);     
    }
  }   

  fetchData(callback);
});
```

## 承诺

只要您的测试返回一个承诺，Jest 就会知道在完成测试之前等待承诺解决。如果承诺被拒绝，测试就会失败。上面的例子是用 promise 写成的，看起来像这样:

```
test('the data is peanut butter', () => {   
  **return** fetchData().then(data => {    
    expect(data).toBe('peanut butter');   
  }); 
});
```

## 异步/等待

只要`async`在函数之前，就可以在测试中使用`try` / `catch`和/或`await`。与 promises 实现一样，这个实现与您在代码中实际使用 async/await 的方式非常相似。“花生酱”的例子应该是这样的:

```
test('the data is peanut butter', **async** () => {   
  **const** data = **await** fetchData();   
  expect(data).toBe('peanut butter'); 
}); test('the fetch fails with an error', **async** () => {  
  expect.assertions(1);   **try** {     
    **await** fetchData();   
  } **catch** (e) {         
    expect(e).toMatch('error');   
  } 
});
```

# 附加功能

我们将关注上面描述的测试——创建包含使用匹配器测试值的测试用例的测试套件。在以后的文章中，在介绍完 Cypress 之后，我们将讨论如何在这些测试中使用 React 测试库来测试 DOM 节点。尽管我们不会全面介绍 Jest，但我想指出 Jest 提供的一些额外功能。

## 快照

> 一个典型的移动应用程序快照测试用例呈现一个 UI 组件，获取一个快照，然后将其与测试中存储的参考快照文件进行比较。如果两个快照不匹配，测试将失败:要么是意外的更改，要么是引用快照需要更新到 UI 组件的新版本。

以上来自 Jest 文档。正如它所描述的那样，快照用于测试 UI 没有发生意外的变化。在 React 中，这通常涉及到创建和存储 React 树的序列化版本的快照，然后可以与该树的未来序列化版本进行比较。

你可以在这里了解更多关于快照测试[的信息。](https://jestjs.io/docs/en/snapshot-testing)

## 安装和拆卸

> 通常，在编写测试时，您有一些设置工作需要在测试运行之前进行，您还有一些完成工作需要在测试运行之后进行。Jest 提供了帮助函数来处理这个问题。

Jest 提供了一组函数来处理测试运行所需的操作，例如创建测试数据和清除测试数据。

您可以在这里了解更多关于设置和拆卸测试[。](https://jestjs.io/docs/en/setup-teardown)

## 嘲弄的

> 模拟函数允许您通过擦除函数的实际实现、捕获对函数的调用(以及在这些调用中传递的参数)、在用`new`实例化时捕获构造函数的实例以及允许在测试时配置返回值来测试代码之间的链接。

在 Jest 中，您可以模拟返回值、函数实现或整个模块，例如 Axios。嘲笑的核心是在更大的范围内隔离不同的元素，并测试它们。

您可以在这里了解更多关于嘲笑[的信息。](https://jestjs.io/docs/en/mock-functions)

接下来，我们将介绍一种不同类型的测试——使用 Cypress 进行端到端测试。在那之后，我们将回到 Jest 和 React Testing Library，通过编写并使用这三种工具实现真实世界的测试。

> **先前的** : [反应中测试，第 2 部分:反应测试库](https://medium.com/javascript-in-plain-english/testing-in-react-part-2-react-testing-library-f32432b93c6c)
> 
> **下一步** : [反应中检测，第 4 部分:酶](https://medium.com/javascript-in-plain-english/testing-in-react-part-4-enzyme-9b030ad616ae)

## 资源

*   [玩笑官方文件](https://jestjs.io/docs/en/getting-started)
*   [Jest-Dom 官方文件](https://github.com/testing-library/jest-dom)