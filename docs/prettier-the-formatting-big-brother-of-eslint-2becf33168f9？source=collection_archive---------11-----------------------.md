# 更漂亮 ESLint 的格式大哥

> 原文：<https://javascript.plainenglish.io/prettier-the-formatting-big-brother-of-eslint-2becf33168f9?source=collection_archive---------11----------------------->

## 通过更漂亮的自动代码格式化增强开发人员的能力

![](img/c33226a87494fa016d165ae776e6e6fa.png)

Photo by [Martin W. Kirst](https://unsplash.com/@nitram509?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 我的旅程

代码格式的一致性是一个很大的挑战。当然，目标是让一切 100%一致。但是，开发代码库的人越多，这就变得越难。仔细而详细的代码审查可以捕捉到其中的一些问题，但是它们需要花费大量的时间来发现和解决。即便如此，总会有一些东西溜走。

[成立 ESLint 对这个](https://medium.com/javascript-in-plain-english/eslint-a-proofreader-for-your-code-cd7e56ae391f)帮助很大。 [ESLint](https://eslint.org/) 有很多代码格式的规则，这些规则包含在 [Airbnb 的 ESLint 配置](https://www.npmjs.com/package/eslint-config-airbnb)中。但这还不够——ESLint 不能格式化一切。也许将来会，但在那之前，漂亮帮我解决了这个挑战。

# 较美丽

[更漂亮](https://prettier.io/)是 ESLint 的格式化大哥。ESLint 主要关注代码质量和一些代码格式，而 Prettier 只关注代码格式。它更加强大，完全可以自动修复，几乎不需要任何配置。

# 更漂亮 vs ESLint

但是 ESLint 也可以在启用正确规则的情况下进行代码格式化。所以乍一看，更漂亮似乎是多余的。但是更漂亮的功能更强大，涵盖了更广泛的代码格式化主题。

例如，ESLint 有一个`max-len`，如果行长度大于 80(如果使用默认配置)，就会出错。因此，下面一行会产生一个错误，因为对象都是在一行中定义的。

```
const badFormat1 = {
  // the object definition below is all in one line
  firstName: 'John', lastName: 'Doe', username: 'johndoe', email: 'johndoe@gmail.com', phone: '+1 (234) 567-8900', age: 32, sex: 'male',
};
```

但是 ESLint 没有办法自动修复这个错误。此外，同一个对象可以用多种方式定义，ESLint 无法修复它。

```
const badFormat2 = {
    firstName: 'John',
    // the object definition below is all in one line
    lastName: 'Doe', username: 'johndoe', email: '[johndoe@gmail.com](mailto:johndoe@gmail.com)', phone: '+1 (234) 567-8900', age: 32, sex: 'male',
};
```

这导致了 ESLint 无法解决的不一致。

但是漂亮的可以。无论我们如何格式化上面的例子，Prettier 都会自动将代码“修饰”成下面这样。

```
const goodFormat = {
    firstName: 'John',
    lastName: 'Doe',
    username: 'johndoe',
    email: '[johndoe@gmail.com](mailto:johndoe@gmail.com)',
    phone: '+1 (234) 567-8900',
    age: 32,
    sex: 'male',
  };
```

这只是一个例子，还有很多能力是 beauty 拥有而 ESLint 没有的。

# 用户化

类似于 ESLint，Prettier 允许你定制格式选项[，比如](https://prettier.io/docs/en/options.html)[线宽，制表符宽度，分号，引号，括号，以及更多的](https://prettier.io/docs/en/options.html)。这些配置保存在`prettier.config.js`配置文件中。

```
// prettier.config.js
module.exports = {
  singleQuote: true,
  arrowParens: 'avoid',
};
```

但这远不及 ESLint 的定制程度。这可以被认为是更漂亮的缺点。事实上，这就是 Prettier 背后的哲学——它是一个固执己见、自我肯定的格式化程序。漂亮的目的是通过提供一个自己的严格的风格标准来停止不同的个人风格观点和争论。虽然有些人可能不喜欢它，但 Prettier 的造型非常受欢迎——每个月有超过 4000 万次安装在各种项目中。

当我第一次搬到漂亮，我也有这种担心。我已经习惯了我的风格，漂亮的风格是新的和不同的。然而几个星期后，我习惯了。而且我再也没想过(想过写这篇文章不算)。

# ESLint 集成

Prettier 也可以集成到 ESLint 中，所以你不需要一个单独的进程来运行它。更漂亮地提供了`[eslint-plugin-prettier](https://www.npmjs.com/package/eslint-plugin-prettier)`和`[eslint-config-prettier](https://www.npmjs.com/package/eslint-config-prettier)`来完成这个任务。漂亮插件使漂亮规则对 ESLint 可用，ESLint 随后通过推荐的扩展启用。

```
// .eslintrc.js
module.exports = {
  extends: [
    'airbnb',
    'plugin:prettier/recommended'
    'prettier/react',
  ],
};
```

不幸的是，这个更漂亮的规则可能与现有的 ESLint 样式规则相冲突。ESLint 更漂亮的配置通过禁用冲突规则解决了这个问题。在上面的例子中，`[airbnb](https://www.npmjs.com/package/eslint-config-airbnb)`扩展包含了 [React ESLint 插件](https://www.npmjs.com/package/eslint-plugin-react)，所以`prettier/react`扩展是禁用这些规则所必需的。漂亮器还为其他 ESLint 插件提供了禁用功能，比如分别带有`prettier/@typescript-eslint`和`prettier/vue`扩展名的`[@typescript-eslint/eslint-plugin](https://www.npmjs.com/package/@typescript-eslint/typescript-estree)`和`[eslint-plugin-vue](https://www.npmjs.com/package/eslint-plugin-vue)`。因为这些扩展禁用了以前扩展的规则，所以请确保将它们放在最后，以便它们可以覆盖以前的扩展。

# 移民

漂亮是完全可以自动修复的！这使得迁移变得轻而易举。使用 Prettier 的 CLI，`npx prettier --check --write`，或者 ESLint 的 auto fix，`npx eslint . --fix`，你所有的错误，哪怕有几万个，都会在瞬间修复。这使得集成更漂亮变得非常简单。

# 最后的想法

漂亮是一个了不起的代码格式化库，因为它可以为你做任何事情。它引入了代码样式标准来确保格式的一致性。它会自动修复不符合标准的代码，以符合该标准。它将原本花在代码格式化上的时间解放出来，专注于产品开发。给漂亮一点的尝试，虽然你可能不喜欢最初的造型变化，你会喜欢你再也不用考虑造型了。

# 资源

*   [官方漂亮文件](https://prettier.io/)
*   [Github 本文回购](https://github.com/mjchang/medium/tree/master/prettier)
*   [本文的 CodeSandbox】](https://codesandbox.io/s/github/mjchang/medium/tree/master/prettier)