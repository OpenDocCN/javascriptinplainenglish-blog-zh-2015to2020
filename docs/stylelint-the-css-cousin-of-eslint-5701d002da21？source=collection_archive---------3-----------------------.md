# stylelint——ESLint 的 CSS 表亲

> 原文：<https://javascript.plainenglish.io/stylelint-the-css-cousin-of-eslint-5701d002da21?source=collection_archive---------3----------------------->

## 校对 CSS 的质量和一致性

![](img/ea91e69bc946eb2ff2851efe8fd813f6.png)

Photo by [Andrew Seaman](https://unsplash.com/@amseaman?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 我的旅程

我第一次了解 Stylelint 是在浏览样式化组件文档时。我并没有特别寻找它，但是因为我刚刚在我的代码库中设置了 [ESLint](https://medium.com/javascript-in-plain-english/eslint-a-proofreader-for-your-code-cd7e56ae391f) 和更漂亮，我被激起了兴趣。从那时起，我开始研究 Stylelint。很快，我意识到，虽然我通过 ESLint 和 Pretty 获得了最终的 Javascript 林挺设置，但我对 CSS 一无所知。很明显，这是我的下一个任务。

# Stylelint

[Stylelint](https://stylelint.io/) 是 CSS 的 linter。它的工作原理是分析你的 CSS，如果任何配置的规则被违反，它会发出警告。这些规则可以捕捉 CSS 错误并强制执行样式约定。它类似于 Google Doc 或 Microsoft Word 的拼写和语法检查——本质上是 CSS 的自动校对工具！

# 配置

Stylelint 运行一个名为`stylelint.config.js`的配置文件。这个配置文件被分解成[规则](#1a0b)、[插件](#393b)和[扩展](#db32)。

# 规则

规则定义了 Stylelint 将在代码中寻找什么。这些在配置的`rules`部分被定义为键值对。键是规则，值切换规则并设置选项。

启用规则取决于规则本身——有些规则可能很简单，只需将值设置为`true`；其他人可以通过关键词直接调谐。

```
// stylelint.config.js
module.exports = {
  rules: {
    'declaration-no-important': true,
    'color-hex-case': 'upper',
  },
};
```

有些规则还允许您使用额外的配置选项对其进行进一步定制。在这种情况下，主要选项和附加配置是一个阵列。

```
module.exports = {
  rules: {
    'font-weight-notation': ['numeric', { ignore: ['relative'] }],
  },
};
```

禁用要简单得多——只需将值设置为`null`。

```
module.exports = {
  rules: {
    'comment-no-empty': null,
  },
};
```

# 插件

Stylelint 本身提供了近 200 条规则，但这些规则并没有涵盖所有内容。这就是插件的用武之地——它们允许开发人员创建规则，然后您可以在配置中启用这些规则。这里有几个 Stylelint 插件的例子。

[](https://www.npmjs.com/package/stylelint-a11y) [## stylelint-a11y

### 可访问性的 Stylelint 规则

www.npmjs.com](https://www.npmjs.com/package/stylelint-a11y) [](https://www.npmjs.com/package/stylelint-scss) [## stylelint-scss

### SCSS 的 Stylelint 规则

www.npmjs.com](https://www.npmjs.com/package/stylelint-scss) [](https://www.npmjs.com/package/stylelint-order) [## stylelint-订单

### CSS 排序的 Stylelint 规则

www.npmjs.com](https://www.npmjs.com/package/stylelint-order) 

要集成，在配置的`plugins`部分定义这些插件。然后你可以在`rules`部分使用插件的规则。

```
module.exports = {
  plugins: [
    'stylelint-a11y',
  ],
  rules: {
    "a11y/no-outline-none": true,
  },
};
```

# 扩展ˌ扩张

如此多的原生规则和插件贡献了更多的规则，Stylelint extensions 使得这些规则更容易使用，因此您不必费力去理解它们。通过扩展，你继承了插件和规则，这样你就不用自己动手了。下面是一些 Stylelint 扩展的例子。

[](https://www.npmjs.com/package/stylelint-config-standard) [## stylelint-配置-标准

### Stylelint 创建者推荐的 Stylelint 配置

www.npmjs.com](https://www.npmjs.com/package/stylelint-config-standard) [](https://www.npmjs.com/package/stylelint-config-recommended-scss) [## stylelint-config-recommended-scss

### 推荐 SCSS 使用的 Stylelint 配置

www.npmjs.com](https://www.npmjs.com/package/stylelint-config-recommended-scss) [](https://www.npmjs.com/package/stylelint-a11y) [## style lint-a11y/推荐

### 可访问性的推荐 Stylelint 配置

www.npmjs.com](https://www.npmjs.com/package/stylelint-a11y) 

扩展在配置的`extensions`部分定义。

```
module.exports= {
  extends: [
    'stylelint-config-standard',
    'stylelint-config-recommended-scss',
    'stylelint-a11y/recommended',
  ],
};
```

# 覆盖+禁用

但是使用扩展会产生一个副作用——如果您大部分同意它们，但是有一些规则您想调整或完全关闭，该怎么办？这就是覆盖和禁用的用武之地。您自己定义的规则将优先考虑并覆盖扩展的规则。

```
module.exports = {
  extends: [
    'stylelint-config-standard',
  ],
  rules: {
    'color-hex-case': 'upper',
    'length-zero-no-unit': null,
  },
};
```

即使你尽最大努力遵守规则，也可能有违反规则的情况。为了防止 Stylelint 出错，可以临时禁用带有注释的特定行的 Stylelint。

*   `/* stylelint-disable */`:禁用下面所有行的 Stylelint，直到用`/* stylelint-enable */`重新启用
*   `/* stylelint-disable-line */`:仅禁用当前行的 Stylelint
*   `/* stylelint-disable-next-line */`:仅对下一行禁用 Stylelint

您还可以在 disable 注释后指定特定的(用逗号分隔的)规则来禁用一些规则，而不是禁用所有规则。

```
div {
  /* stylelint-disable-next-line value-no-vendor-prefix */
  display: -webkit-flex;
  justify-content: center;
  align-items: center;
  font-size: 12px !important; /* stylelint-disable-line declaration-no-important */

  /* stylelint-disable */
  :focus {
    outline: none;
  }
  /* stylelint-enable */
}
```

应该谨慎使用这些规则，所以如果您经常禁用这些规则，请考虑永久关闭它们，

# JS 中的 CSS

如果你在 JS 框架中使用流行的 CSS，比如 [Emotion](https://emotion.sh/docs/introduction) 或者 [Styled Components](https://styled-components.com/) ，你仍然可以在一点额外的配置后 lint 你的 CSS。`stylelint-processor-styled-components`处理器将从情感或样式化组件中提取样式，这样 Stylelint 就可以处理它们。此外，某些规则与这些框架不兼容，因此必须使用`stylelint-config-styled-components`将其关闭。

```
module.exports= {
  extends: ['stylelint-config-styled-components'],
  processors: ['stylelint-processor-styled-components'],
};
```

# 多种配置

如果您使用多个 CSS 框架，您可能需要多个配置，每个配置针对一个特定的框架。例如，如果您使用普通 CSS 和样式化组件，由于两种配置之间的差异，您将需要为每种组件配置一个配置。

```
// stylelint.config.js
module.exports = {
  extends: [
    'stylelint-config-standard',
    'stylelint-a11y/recommended'
  ],
};// stylelint.styled.config.js
module.exports = {
  extends: [
    './stylelint.config.js',
    'stylelint-config-styled-components'
  ],
  processors: ['stylelint-processor-styled-components'],
};
```

快速提示:你可以让你的配置互相扩展，这样你就不需要再定义相同的规则，插件扩展了！

# 运转

为了运行 Stylelint(使用多种配置)，我向`package.json`添加了以下别名。

```
{
  "scripts": {
    "lint:js": "stylelint '{**/*,*}.js' --config stylelint.styled.config.js",
    "lint:css": "stylelint '{**/*,*}.css'",
    "lint:css:fix": "stylelint '{**/*,*}.css' --fix"
  },
}
```

`npm run lint:js`将对样式化组件(扩展名为`.js`的文件)运行 Stylelint，`npm run lint:css`将对普通 CSS(扩展名为`.css`的文件)运行 Stylelint。此外，Stylelint 支持自动修复，因此您可以运行`npm run lint:css:fix`，任何可以自动修复的 Stylelint 违规都会被修复。不幸的是，当您使用样式组件所需的处理器时，不支持自动修复。

# 最后的想法

Stylelint 是对 ESLint 执行和维护高质量代码库的完美补充。虽然 Stylelint 最初的设置有点复杂，但完成后，您可以对 CSS 的质量和一致性充满信心。这只是少了一件你需要担心的事情，所以你可以专注于构建一个伟大的产品。

# 资源

*   [官方 Stylelint 文档](https://stylelint.io/)
*   [本文 Github 回购](https://github.com/mjchang/medium/tree/master/stylelint)
*   [本文的 code sandbox](https://codesandbox.io/s/github/mjchang/medium/tree/master/stylelint)