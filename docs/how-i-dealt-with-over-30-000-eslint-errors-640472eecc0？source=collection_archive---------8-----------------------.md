# 我如何处理超过 30，000 个 ESLint 错误

> 原文：<https://javascript.plainenglish.io/how-i-dealt-with-over-30-000-eslint-errors-640472eecc0?source=collection_archive---------8----------------------->

## ESLint —迁移到综合配置

![](img/8f09b0ee70535db2c75a546a7de6b6d9.png)

Photo by [Rohan Reddy](https://unsplash.com/@rofotoqoto?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 我的旅程

当我第一次了解 ESLint 的时候，我非常兴奋能够在我的代码库中启用它。我渴望获得支持 ESLint 的代码库提供的所有好处。然而，这涉及到相当密集的迁移，以将我的代码库升级到我的 ESLint 配置标准。这是我的经历。

# 我的配置

我研究了很多，并最终决定采用这种配置。

```
module.exports = {
  extends: [
    'airbnb',
    'airbnb/hooks',
    'plugin:jest/recommended',
    'plugin:jest/style',
    'plugin:prettier/recommended',
    'plugin:testing-library/react',
    'prettier/react',
  ],
};
```

[Airbnb 的 ESLint](https://www.npmjs.com/package/eslint-config-airbnb) 似乎是 Javascript 社区中事实上的编码标准，自从我开始使用 React Hooks 以来，我也包括了 Airbnb 的 Hooks 规则。我使用了 [Jest](https://jestjs.io/) 和 [React 测试库](https://testing-library.com/docs/react-testing-library/intro)，所以包含他们推荐的 ESLint 配置是有意义的。最后，我加入了更漂亮的 T8 和 T9，因为我听说了关于它如何从根本上消除了思考和担心代码格式的需求的好消息。

# 移民

有了这个配置，我第一次运行了 ESLint。哦，天啊，我的肚子上挨了一拳。我的代码库有超过 30，000 个错误！我做了什么！？

经过几分钟的震惊、怀疑和后悔我的主动，我运行了 ESLint 的自动修复。这次我只有大约 5000 个错误。嗯，这样好多了。自动修复修复了 25，000 个错误，但我仍然有大量的问题需要手动解决。

# 关闭规则

在这一点上，我只需要坐下来一个一个地解决它们。我首先列出了每一条被违反的规则。然后，我把所有这些规则都关掉了。就这样——我通过了棉绒！

不完全是。虽然我的代码看起来不错，因为 ESLint 没有抛出任何错误，但我只是通过禁用规则来掩盖这个问题。但这是有意的——我想一次处理一条规则的违反，而不被所有的错误一次分散注意力和过载。总而言之，我已经禁用了 50 多条规则。现在真正的工作开始了——让他们一个接一个地修复错误。

# 那些简单的

一些规则非常容易修改。诸如`no-unused-vars`、`no-constant-condition`或`react/no-unused-state`之类的规则涉及简单地删除有问题的代码，而不必理解代码。要是有更多这样的简单修复就好了。

# 那些复杂的

有些规则需要复杂得多。像`import/cycle`这样的规则，让我重新组织导出和导入以避免循环依赖，需要很大的努力才能解决。这使得意外错误和回归的风险更高，因此还需要额外的努力来验证变更。虽然我不喜欢每次违反规则所付出的努力，但我不得不低下头去做。修正这些规则无疑是整个努力的低谷。

# 我从他们身上学到的

一些规则让我受益匪浅。特别是，我从`jsx-a11y`规则中学到了可访问性。像`jsx-a11y/no-static-element-interactions`、`jsx-a11y/interactive-supports-focus`和`jsx-a11y/click-events-have-key-events`这样的规则告诉我，像 div 这样有一些交互(例如`onClick`)的静态元素需要有一个`role`、`tabIndex`和一个键盘事件，这样辅助技术才能与元素交互。这是一个我没有预料到的附带好处。

# 那些需要调整的

有些规则是不可避免的。但幸运的是，他们提供了配置选项，允许我所需要的，所以我不需要永久关闭规则。

```
module.exports = {
  rules: {
    'no-empty': ['error', { allowEmptyCatch: true }],
    'no-underscore-dangle': ['error', { allow: ['_id'] }],
    'react/jsx-filename-extension': [
      'error',
      { extensions: ['.js', '.jsx'] }
    ],
  },
}
```

# 需要内联的会禁用

一些规则在原则上是好的，但在某些情况下需要禁用它们。如果你将道具扩展到 JSX，那么`react/jsx-props-no-spreading`规则会出错，但是这对于高阶组件和基于钩子的库是必要的，比如 [React Dropzone](https://github.com/react-dropzone/react-dropzone) 或 [React Table](https://github.com/tannerlinsley/react-table) 。因为我只希望在少数情况下需要适当的传播，所以我选择了内联禁用规则，而不是永久禁用。

```
const withAuth = (WrappedComponent, permissions) => {
  const PermissionCheck = props => {
    const hasPermission = ...;
    if (hasPermission) {
      /* eslint-disable-next-line react/jsx-props-no-spreading */
      return <WrappedComponent {...props} />;
    }
    return <Redirect to="/" />;
  };
  return PermissionCheck;
};

export default withAuth;
```

# 那些暂时被禁用的

有些规则我很想启用，但是一次完成要花很长时间，因为它们占了大部分错误。作为临时措施，我会暂时禁用它们，然后一个文件夹一个文件夹地慢慢修复错误，直到有一天，希望这些规则可以永久启用。

ESLint 提供了一个`overrides`部分，可以定位文件并对这些文件应用特定的规则。使用这一点，我禁用了整个文件夹，后来，我删除了禁用一个接一个，并解决了错误。

```
module.exports = {
  overrides: [
    {
      // todo: slowly enable
      files: [
        'src/views/dashboard/**/*.js',
        'src/views/users/**/*.js',
        'src/views/settings/**/*.js',
      ],
      rules: {
        'react/destructuring-assignment': 'off',
        'react/prop-types': 'off',
      },
    },
  ],
}
```

# 那些永久残疾的人

幸运的是，我不认为任何规则保证这一点。确实有一些令人头疼的规则，但我理解它们背后的原因。我想尽最大努力遵循 Airbnb 编码标准和框架建议，这意味着保持所有规则的启用。

# 整体流程

总之，完全集成我的 ESLint 配置花了将近一个月的时间。但不要让这吓走你！解决简单的问题和微调规则只花了一天时间。更复杂、更费力的项目需要大约一周时间。其中，80%的 ESLint 错误得到了解决。

剩下的就是我暂时禁用的有问题的文件夹。这些，我花了我的时间。因为我只是逐个文件夹地禁用它们，所以不在这些文件夹中的任何新代码都将应用这些规则。我觉得，这是完成 ESLint 迁移和产品特性开发的时间压力之间的一个很大的妥协。

# 最后的想法

虽然移植 ESLint 配置的整个过程很漫长，但我很高兴我做到了。由于这种最初的努力——设置适当的 ESLint 配置并修复代码库——我们不再需要考虑代码格式或花费时间担心代码质量，因此，我们可以将所有时间用于提高工作效率。为了内心的平静，努力是值得的。

# 资源

*   [ESLint 官方文档](https://eslint.org/)
*   [建立 ESLint](https://medium.com/javascript-in-plain-english/eslint-a-proofreader-for-your-code-cd7e56ae391f)