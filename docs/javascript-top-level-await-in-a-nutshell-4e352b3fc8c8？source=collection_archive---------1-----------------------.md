# JavaScript 中新的“顶级等待”特性是如何工作的

> 原文：<https://javascript.plainenglish.io/javascript-top-level-await-in-a-nutshell-4e352b3fc8c8?source=collection_archive---------1----------------------->

## 简短、有用的 JavaScript 课程——让它变得简单。

![](img/12d06bfd605bebef9e6d708582a7427e.png)

Photo by John Petalcurin

以前，为了使用 await，代码需要位于标记为 async 的函数中。这意味着您不能在任何函数符号之外使用 await。顶级 await 使模块能够像异步函数一样工作。

模块是异步的，有导入和导出，这些也在顶层表达。这实际上意味着，如果你想提供一个依赖于一些异步任务的模块来做一些事情，你真的没有什么好的选择。

顶级 await 解决了这个问题，并使开发人员能够在异步函数之外使用 await 关键字。使用顶级 await，ECMAScript 模块可以等待资源，导致导入它们的其他模块在开始评估它们的主体之前等待，或者如果模块加载失败，您也可以使用它作为加载依赖项的后备，或者使用它来加载下载的第一个资源。

注意事项:

*   顶层等待**只在模块**的顶层工作。不支持经典脚本或非异步函数。
*   在撰写本文时(2020 年 2 月 23 日)，ECMAScript 阶段 3。

# 用例

使用顶级 await，下一个代码在模块中的工作方式与您预期的一样

# 1.如果模块加载失败，使用回退

以下示例尝试从 first.com 加载 JavaScript 模块，如果失败，则返回:

```
//module.mjslet [module](https://first.com/libs.com/lib1');try {
  [module](https://first.com/libs.com/lib1')= await import('[https://first.com/libs.com/module1'](https://first.com/libs.com/lib1'));
} catch {
  [module](https://first.com/libs.com/lib1')= await import('[https://second.com/libs/](https://second.com/libs/lib1')[module1](https://first.com/libs.com/lib1')['](https://second.com/libs/lib1'));
}
```

# 2.使用加载最快的资源

在这里，res 变量通过先完成下载的变量进行初始化。

```
//module.mjsconst resPromises = [    
    donwloadFromResource1Site,
    donwloadFromResource2Site
 ];const res = await Promise.any(resPromises);
```

# 3.资源初始化

顶级 await 允许您在模块中等待承诺，就像它们被包装在异步函数中一样。例如，这对于执行应用程序初始化非常有用:

```
//module.mjsimport { dbConnector} from './dbUtils.js'//connect() return a promise.
const connection = await dbConnector.connect();export default function(){connection.list()}
```

# 4.动态加载模块

这允许模块使用运行时值来确定依赖性。

```
//module.mjsconst params = new URLSearchParams(window.location.search);
const lang = params.get('lang');
const messages = await import(`./messages-${lang}.mjs`);
```

# 5.在 DevTools 中使用 await 外部异步函数？

在使用 async/await 之前，试图在异步函数之外使用 await 会导致“语法错误:await 仅在异步函数中有效”现在，您可以在异步函数中不使用它的情况下使用它。

这已经在 chrome 80 和 firefox 72.0.2 **DevTools** 中测试过了。然而，这个功能是非标准的，在 nodejs 中不起作用。

```
const helloPromise = new Promise((resolve)=>{
  setTimeout(()=> resolve('Hello world!'), 5000);
})const result =  await helloPromise;console.log(result);//5 seconds later...:
//Hello world!
```

# 履行

*   带旗帜的 V8 发动机:— —和谐——顶级——等待
*   web pack(5 . 0 . 0 中的实验性支持)
*   Babel([Babel/plugin-syntax-top-level-await](https://babeljs.io/docs/en/babel-plugin-syntax-top-level-await))增加了解析器支持。

# 参考

*   [https://github.com/bmeck/top-level-await-talking/](https://github.com/bmeck/top-level-await-talking/)
*   [https://github.com/tc39/proposal-top-level-await#use-cases](https://github.com/tc39/proposal-top-level-await#use-cases)

感谢你阅读我！！