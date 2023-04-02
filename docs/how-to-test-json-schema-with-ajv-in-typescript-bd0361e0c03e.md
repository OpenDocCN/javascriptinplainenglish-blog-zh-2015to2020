# 如何在 TypeScript 中用 AJV 测试 JSON 模式

> 原文：<https://javascript.plainenglish.io/how-to-test-json-schema-with-ajv-in-typescript-bd0361e0c03e?source=collection_archive---------1----------------------->

![](img/d8d8e6a5c1baaeca6cb74b24114818a1.png)

如何测试您的请求和响应是否与接口匹配？我初学`Jest`的时候，只用过`toMatchSnapshot`。使用`toMatchShot`，如果没有现有的快照，您可以保留传递到`expect`中的数据的快照。如果有，那么`Jest`会自动将现有值与您传递给`expect`的值进行比较。

当然，快照测试是很棒的，尤其是因为你的 UI 组件不会发生意外的变化。然而，如果你处理的数据经常改变，你不能保证它总是满足你的测试用例。

# TL；速度三角形定位法(dead reckoning)

*   测试 API 响应并不容易，因为它的值总是变化的。因此，我们可以检查值的类型，而不是比较值。
*   `typescript-json-schema`用于生成 JSON 模式对象。
*   `ajv`用于通过与用`typescript-json-schema`创建的 JSON 模式对象进行比较来验证您的数据

# JSON 模式将是您的解决方案之一

> **JSON 模式**是一个词汇表，允许您**注释**和**验证** JSON 文档。

JSON Schema 是一个基于 JSON 的格式数据文档。易于维护、阅读和使用。

假设您拥有一个 JSON 对象。

```
{
  "name": "john",
  "age": 30
}
```

这个 JSON 对象在通过 JSON 模式生成器时被转换如下。

```
{
  "definitions": {},
  "$schema": "http://json-schema.org/draft-07/schema#",
  "$id": "http://example.com/root.json",
  "type": "object",
  "title": "The Root Schema",
  "required": [
    "name",
    "age"
  ],
  "properties": {
    "name": {
      "$id": "#/properties/name",
      "type": "string",
      "title": "The Name Schema",
      "default": "",
      "examples": [
        "john"
      ],
      "pattern": "^(.*)$"
    },
    "age": {
      "$id": "#/properties/age",
      "type": "integer",
      "title": "The Age Schema",
      "default": 0,
      "examples": [
        30
      ]
    }
  }
}
```

是的，没错。你们中的一些人可能已经注意到它看起来有点像[](https://en.wikipedia.org/wiki/Abstract_syntax_tree)*。花一点时间来看看这个结构。你能看到什么？对象中几乎没有属性。*

*我们要关注的是*属性*。它包含原始对象中的所有关键点，包括*姓名*和*年龄*。`*type*`描述物业应满足的类型。请注意，在验证我们将要研究的 JSON 模式时，了解这一点非常重要。*

# *好了，我知道 JSON 模式是什么了。"那我如何将我的 typescript 接口转换成 JSON 模式呢？"*

*好吧。有一个非常有用的库可以将 typescript 接口转换成 JSON 模式文件。 [*点击此处访问 TJS*](https://github.com/YousefED/typescript-json-schema#readme) 的 github 页面。*

*推荐你先看一下 [*这个帖子*](https://medium.com/@moonformeli/jest-how-to-use-extend-with-typescript-4011582a2217) ，里面讲的是`Jest.extend`。这篇文章的前提是，你已经知道如何在`Jest`中定制匹配器。*

## *步骤 1:创建项目结构*

*让我们首先创建项目。*

```
*> npm init -y
> mkdir src*
```

*然后，安装所需的软件包。*

```
*> npm i -D jest ts-jest typescript @types/jest
> touch jest.config.js
> touch ./src/tsconfig.json*
```

**tsconfig.json* 文件应该包含这些选项。*

*这里是为了 *jest.config.js**

*创建测试和打字文件夹。*

```
*> mkdir ./src/typings ./src/__tests__
> touch ./src/typings/index.d.ts*
```

*我们现在要做的是为 typescript 创建类型化文件，这样我们就可以稍后在测试套件中使用它。用`index.d.ts`写代码。*

*为了让`Jest`知道我们已经准备好注册我们的新匹配器，创建新文件夹并制作一个文件。我们将在测试文件中导入这个文件，并在所有测试用例之前调用函数。*

```
*> mkdir ./src/hooks
> touch ./src/hooks/index.ts*
```

*`extendJSCMatcher`尚未完成，我们稍后将完成该功能。*

*最后，创建测试文件，基本的准备工作就完成了。*

```
*> touch ./src/__tests__/jsc.spec.ts*
```

## *步骤 2:创建接口和包*

*如果到目前为止你按照步骤做得很好，这个项目应该像下面这样。*

```
*node_modules
src
  __tests__
    jsc.spec.ts
  hooks
    index.ts
  typings
    index.d.ts
jest.config.js
package-lock.json
package.json*
```

*由于我们将使用节点本地模块( *path* 和 *fs* ，我们需要安装某种类型的定义包。此外，这里是我们的英雄，JSON 模式验证器和 JSON 模式生成器包。*

```
*> npm i -D @types/node typescript-json-schema ajv*
```

*只要使用 typescript，就要安装`@types/node`包使用 *path* 或者 *fs* 等节点原生模块。 [*点击此处查看更多关于@types/node*](https://www.npmjs.com/package/@types/node) *。**

*`typescript-json-schema`是为 JSON 模式文件生成 typescript 接口的神奇库。即使指南不是很好，你也可以写一个简单的例子。*

*`ajv`是 Node.js 和浏览器最快的 JSON 模式验证器。它做的和`typescript-json-schema`做的完全相反。`ajv`根据给定的模式，检查给定的对象是否编写正确。如果对象与模式不匹配，它将返回一个错误对象。*

*现在我们需要为接口创建文件夹。*

```
*> mkdir ./src/models
> touch ./src/models/ISchool.ts*
```

*接口文件中将有 3 个有效载荷。*

## *步骤 3:编写 JSON 模式生成代码*

*使*实用程序*的文件夹层次如下。*

```
*utils
  index.ts
  jsc.ts
  validate.ts*
```

*首先，让我们从 *index.ts* 开始*

*很简单的管道函数，但是如果你不懂这个或者不知道什么是管道函数，我推荐你 [*看这篇文章*](https://medium.com/@venomnert/pipe-function-in-javascript-8a22097a538e) 。注意，这里没有返回值。我们将在不同的文件中调用该函数。*

*现在，我们来谈谈套餐，`typescript-json-schema`。有几个步骤你需要记住。*

```
*1\. import the package
2\. generate the object called program
3\. generate the object called generator, using program*
```

*整个画面看起来很简单。对于#2 和#3，只需要传递几个参数。*

*`generator`的方法很少。其中一个是 *getUserSymbols* ，它获取所有的符号。列表中有很多符号，如果你运行`console.dir(generator.getUserSymbols())`，看看最上面的 3 个项目。*

```
*// all symbols
[ 'StudentInterface',
  'TeacherInterface',
  'SchoolInterface',
  'BaseComment',
  'CommentBlock',
  ...
]*
```

*它们是你之前写的 typescript 接口。这是因为你在程序生成器函数中添加了额外的文件。*

```
*console.log(files);
// ['..yourPath/ISchoolts']*
```

*这次试试`console.log(generator.getSchemaForSymbol(‘StudentInterface’))`，看看之后会怎么样。*

```
*// this is the schema object
{ type: 'object',
  properties:
   { name: { type: 'string' },
     age: { type: 'number' },
     address: { type: 'string' } },
  required: [ 'age', 'name' ],
  '$schema': '[http://json-schema.org/draft-07/schema#'](http://json-schema.org/draft-07/schema#') 
}*
```

*现在您已经创建了模式对象！*

*最后，您可以将这些模式对象存储在目录 *schema* 中。*

## *步骤 4:用 ajv 验证 JSON 模式*

*这将是最后一步。既然我们在 *schema* 文件夹中创建了 JSON 模式文件，那么是时候使用`ajv`了。*

*打开 *validate.ts* 文件，编写这段代码。*

*还有几个使用`ajv`、[、*的方法点击这里查看。*](https://www.npmjs.com/package/ajv#options) 当*验证*未发现错误时，则 *errorText()* 的错误信息为“无错误”。*

*运行这个文件，看看会得到什么结果。*

```
*> npx ts-node ./src/utils/validate.ts// result
> data should have required property 'subject'*
```

*尝试其他测试对象，看看结果！*

*我们忘了完成我们制作的 jest hook 文件。这实际上是为了测试代码，所以如果您不想运行测试，可以跳过这一步。*

# *完整代码*

*我在这里上传了完整的代码[](https://github.com/moonformeli/typescript-json-schema-validate)**稍加重构。***