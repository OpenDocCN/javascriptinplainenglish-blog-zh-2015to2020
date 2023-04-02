# 以正确的方式捆绑 monorepos

> 原文：<https://javascript.plainenglish.io/bundling-monorepos-the-right-way-34116aa50433?source=collection_archive---------1----------------------->

## 了解如何使用 RollupJS 和 Lerna API！

## Lerna 是一个很好的工具，可以在一个存储库中保存多个包。

然而，它没有为自动化构建过程提供内置的解决方案。在这里，我描述了如何创建一个使用 RollupJS+Lerna API 的构建脚本来:

*   按照依赖关系的顺序编译包
*   *仅重建已经更改的包*
*   *将多个包捆绑成一个“超级包”供用户使用——这样他们就不必构建整个项目。*

*![](img/1bb044ec5adfe981ef84ae16ded482c2.png)*

*Photo by [Kevin Ku](https://unsplash.com/@ikukevk?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)*

# *设置*

*您的`rollup.config.js`应该在运行时生成一个对象，而不是导出一个硬编码的静态配置对象。该文件的框架如下所示:*

```
*async function main() {
  return {
    /* ... */
  };
}export default main();// @returns Promise<RollupConfig>*
```

## *安装 Lerna API*

*要创建一个包列表并按照依赖关系对它们进行排序，我们可以使用 Lerna 的内部 API:*

```
*npm install -D @lerna/project @lerna/filter-packages @lerna/batch-packages*
```

*然后，向您的构建脚本`rollup.config.js`添加一个助手函数:*

```
*import { getPackages } from '@lerna/project';
import filterPackages from '@lerna/filter-packages';
import batchPackages from '@lerna/batch-packages';/**
 * @param {string}[scope] - packages to only build (if you don't
 *    want to build everything)
 * @param {string}[ignore] - packages to not build
 *
 * @returns {string[]} - sorted list of Package objects that
 *    represent packages to be built.
 */
async function getSortedPackages(scope, ignore) {    
  const packages = await getPackages(__dirname);    
  const filtered = filterPackages(packages,
    scope,
    ignore,
    false,);     

  return batchPackages(filtered)
    .reduce((arr, batch) => arr.concat(batch), []);
}*
```

*这个函数有两个(可选的)参数— `scope`和`ignore`。如果你想**只**构建一些包，你可以在这个字符串中传递它们的名字。如果你想**而不是**构建一些包，同样，你可以在这个字符串中传递它们的名字。*

*通过不在配置文件中硬编码包名并使用这个函数来生成它们，重构 monorepo 不需要更新`rollup.config.js`！*

# *生成配置对象*

## *package.json 中的主/模块/包字段*

*这个例子期望您的每个包`package.json`都有一个`main`字段指向它们的 CommonJS 包。如果还没有，请务必添加它们。*

*此外，如果您正在构建 ESM 包，应该添加`module`字段。`bundle`字段可用于生成生命/UMD 包。*

## *输入文件*

*这个例子还假设您的输入文件在您所有的包中都是`src/index.js`。如果这对于您的每个包都是不同的，那么您可能想要在每个包的`package.json`文件中添加一个`input`字段。*

## *返回主页面()*

*`main`函数应该为每个包*按顺序*创建一个包含配置对象的数组:*

> *`npm install minimist` —我们使用 minimist 包来解析命令行参数(用于范围和忽略)。*

```
*import path from 'path';
import minimist from 'minimist';async function main() {
  const config = []; // Support --scope and --ignore globs if passed in via commandline
  const { scope, ignore } = minimist(process.argv.slice(2)); const packages = await getSortedPackages(scope, ignore); packages.forEach(pkg => {
    /* Absolute path to package directory */
    const basePath = path.relative(__dirname, pkg.location); /* Absolute path to input file */
    const input = path.join(basePath, 'src/index.js'); /* "main" field from package.json file. */
    const { main } = pkg.toJSON(); /* Push build config for this package. */
    config.push({
      input,
      output: [{
        file: path.join(basePath, main),
        format: 'cjs',
        source map: true
      }, /* Add any other configs (for esm or iife format?) */]
  }); return config;
}*
```

*`rollup -c`现在应该构建所有的包了。*

*另外，`rollup -c --scope @ns/pkgname`应该构建一个包，`rollup -c --ignore @ns/pkgname`应该*

# *超级捆绑包*

*创建一个从 monorepo 中导出所有内容的“超级捆绑”包的最简单方法是创建另一个包来重新导出所有包。*

> *`lerna create bundles/superbundle` —这将在`bundles`文件夹中创建一个 Lerna 包。*
> 
> *创建“超级捆绑包”之后，确保将 monorepo 中的所有其他包作为“超级捆绑包”的依赖项添加进来。你可以通过运行`lerna add @pkg/name1 @pkg/name2 @pkg/name3`来实现。*
> 
> *将“主/模块/包”字段添加到`package.json`以生成输出。*

*超级包应该有一个类似如下的输入文件`src/index.js`:*

```
*import * as name1 from '@pkg/name1';
import * as name2 from '@pkg/name2';
import * as name3 from '@pkg/name3';/* If you want to export in a namespace */
export const NS = {
 ...name1,
 ...name2,
 ...name3
};/* If you want to expose everything directly */
export * from '@pkg/name1';
export * from '@pkg/name1';
export * from '@pkg/name1';*
```

*运行`rollup -c`应该也会自动构建超级包！*

*嘿，伙计们，我是 Shu Kant Pal——pix ijs 的核心维护者和 Silcos 内核的创建者。喜欢我的内容？，跟我来这里:*

*[](https://twitter.com/ShukantP) [## 舒坎特·库马尔·帕尔

### Shukant Kumar Pal (@ShukantP)的最新推文。PixiJS 项目成员，pixi-UI/PuxiJS 的维护者。自我…

twitter.com](https://twitter.com/ShukantP) 

p . s——pixi js 使用这里描述的构建系统！

## 进一步阅读

[](https://bit.cloud/blog/painless-monorepo-dependency-management-with-bit-l4f9fzyw) [## 使用 Bit 进行无痛 monorepo 依赖管理

### 简化 monorepo 中的依赖关系管理，以避免虚拟依赖关系和版本问题。了解…

比特云](https://bit.cloud/blog/painless-monorepo-dependency-management-with-bit-l4f9fzyw) 

*更多内容请看*[***plain English . io***](https://plainenglish.io/)*。报名参加我们的* [***免费周报***](http://newsletter.plainenglish.io/) *。关注我们关于*[***Twitter***](https://twitter.com/inPlainEngHQ)[***LinkedIn***](https://www.linkedin.com/company/inplainenglish/)*[***YouTube***](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw)***，以及****[***不和***](https://discord.gg/GtDtUAvyhW) *对成长黑客感兴趣？检查* [***电路***](https://circuit.ooo/) ***。******