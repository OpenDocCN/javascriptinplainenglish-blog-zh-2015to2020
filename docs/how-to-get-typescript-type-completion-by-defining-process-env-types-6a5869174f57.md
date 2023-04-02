# 如何为 process.env 定义 TypeScript 类型以实现自动完成

> 原文：<https://javascript.plainenglish.io/how-to-get-typescript-type-completion-by-defining-process-env-types-6a5869174f57?source=collection_archive---------1----------------------->

![](img/e9a7fcca6c9140080b6b006abe1db064.png)

> 在 NodeJS 程序中使用`process.env`函数来消耗环境变量是非常频繁的，这是[十二因子应用](https://12factor.net/config)的一个良好实践，这是设计自动扩展应用的一个清单。

默认情况下，当您在 Javascript 项目中安装 NodeJS TS 环境类型(`@types/node`)时，`process.env`类型定义如下:

```
interface Process extends EventEmitter {
  emitWarning(warning: string | Error, name?: string, ctor?: Function): void;
  env: ProcessEnv;
  exit(code?: number): never;
}

interface ProcessEnv {
  [key: string]: string | undefined;
}
```

我们看到流程对象有一个`ProcessEnv`(一个接口)类型的`env`属性。该接口由一个带有字符串键和字符串或未定义值的对象组成。

**太好了！**键入`process.env.HOST`typescript 编译器不会尖叫，另一方面强制检查注入的环境变量，知道哪些变量可用，不太爽……

**你会同意:我们可以获得更好的开发体验。**

Typescript 允许通过使用一个叫做[接口合并](https://www.typescriptlang.org/docs/handbook/declaration-merging.html#merging-interfaces)的概念非常简单地覆盖另一个模块的环境定义，由于这个概念，我们将能够在上游定义`process.env`中可用的值的表示。

# 覆盖 NodeJS 中的流程定义以实现自动完成

在我选择的一个名为`modules.d.ts`的文件中定义了你的项目根文件夹，我们将定义下面的东西。

```
declare namespace NodeJS {
  export interface ProcessEnv {
    HOST: string;
    DB_URL: string;
    DB_NAME?: string;
  }
}
```

**警告:**这个模块必须在使用`include`属性的`tsconfig.json`中定义的项目范围内，这样编译器才能找到并考虑到它。

根据 typescript doc 的定义，默认情况下所有的`.d.ts`都包括在内。

> 如果`"files"`和`"include"`都没有指定，编译器默认包含包含目录和子目录中的所有 TypeScript ( `.ts`、`.d.ts`和`.tsx`)文件，使用`"exclude"`属性排除的除外。

或者您可以使用`types`属性手动指定它们，但是您失去了从类型中自动查找的好处，默认情况下查找所有的`@types`文件夹。**这就是为什么我建议不要使用** `**types**` **键。**

```
// Avoid that if possible
{
  // …
  "types": ["@types/node", "modules.d.ts"]
}
```

**警告 2:** 由于 env 变量的性质，所有值都是字符串，您应该在代码中处理转换。

到目前为止，您的编译器和 IDE 应该为您提供了自动补全和良好的验证。

[**🇫🇷STOP！你是法国人吗🥖？**您也可以访问 ici 网站，接收法国的私人简讯🙂](https://codingspark.io)