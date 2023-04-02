# 用 Cucumber 和 WebDriverJS 进行浏览器自动化测试

> 原文：<https://javascript.plainenglish.io/cucumber-and-webdriverjs-3a0ee227c297?source=collection_archive---------5----------------------->

![](img/6f0580c6efcadfe1064523c2d875a9f0.png)

# 背景

来自 [Cucumber.js](https://github.com/cucumber/cucumber-js) 自述:

> 黄瓜是一个用简单语言编写的自动化测试工具。

换句话说，Cucumber 使用[小黄瓜](https://cucumber.io/docs/gherkin/)语法帮助完成[行为驱动开发(BDD)](https://en.wikipedia.org/wiki/Behavior-driven_development) 。

# 先决条件

*   [Node.js](http://b.remarkabl.org/nodejs-site) 和 [npm](http://b.remarkabl.org/get-npm)
*   [火狐](http://b.remarkabl.org/firefox)和[壁虎](http://b.remarkabl.org/geckodriver)

如果你在 macOS 上，你可以用[家酿](http://b.remarkabl.org/homebrew)安装必备软件:

```
$ brew install node
$ brew cask install firefox
$ brew install geckodriver
```

# 安装

安装[黄瓜](https://www.npmjs.com/package/cucumber)和[硒网驱动](http://b.remarkabl.org/1Tx8tt4):

```
$ npm install cucumber@6 selenium-webdriver
```

我们使用的版本是:

```
$ npm ls --depth=0
├── cucumber@6.0.5
└── selenium-webdriver@4.0.0-alpha.7
```

# 设置

创建一个名为`cucumber.js`的文件:

```
$ touch cucumber.js
```

添加内容:

运行`cucumber-js`以查看没有找到任何场景/步骤。

```
$ npx cucumber-js0 scenarios
0 steps
0m00.000s
```

# 特征

创建一个名为`features`的目录:

```
$ mkdir features
```

> 此处目录名的拼写和大小写必须准确。

在目录中创建一个`.feature`文件。我们将把它命名为`google-search.feature`:

```
$ touch features/google-search.feature
```

编写场景:

运行`npx cucumber-js`查看警告:

这是意料之中的，因为[步骤定义](https://github.com/cucumber/cucumber-js/blob/v6.0.5/docs/support_files/step_definitions.md)未定义。

# 步骤定义

在`features`目录中为您的步骤创建一个`.js`文件。我们将它命名为`google-search-steps.js`:

```
$ touch features/google-search-steps.js
```

从`cucumber`导入`Given`、`When`和`Then`，并粘贴之前的步骤:

运行`npx cucumber-js`查看警告:

# Selenium WebDriver

用`features/google-search-steps.js`中的 WebDriverJS 动作填写[步骤定义](https://github.com/cucumber/cucumber-js/blob/v6.0.5/docs/support_files/step_definitions.md)。

## 在此之前

在`Given`步骤之前构建驱动程序:

## 毕竟

退出`AfterAll`中的驱动[勾](https://github.com/cucumber/cucumber-js/blob/v6.0.5/docs/support_files/hooks.md):

## 考虑到

填写`Given`步骤:

## 当...的时候

填写`When`的步骤:

> 建议使用[等待到](https://www.selenium.dev/selenium/docs/api/javascript/module/selenium-webdriver/lib/until.html)而不是[休眠](https://www.selenium.dev/selenium/docs/api/javascript/module/selenium-webdriver/lib/webdriver_exports_WebDriver.html#sleep)，这样测试运行速度更快，不会变得不稳定。

## 然后

填写`Then`步骤:

运行`npx cucumber-js`查看故障:

# 超时

[超时](https://github.com/cucumber/cucumber-js/blob/6.x/docs/support_files/timeouts.md#timeouts)可以通过两种方式指定:

1.  全球的
2.  台阶(或钩子)

## 全局超时

要指定全局超时:

## 步骤超时

要指定步骤超时:

> 请注意，如何将包含超时的对象指定为 step 函数的第二个参数。

运行`npx cucumber-js`来验证场景是否通过:

```
$ npx cucumber-js
...1 scenario (1 passed)
3 steps (3 passed)
0m06.881s
```

成功！

# 最后的步骤

最后的`features/google-search-steps.js`是这样的:

# 资源

查看 [WebDriverJS 食谱](https://b.remarkabl.org/2eUH7z7)获取更多示例。

## 黄瓜

*   [黄瓜:10 分钟教程](https://cucumber.io/docs/guides/10-minute-tutorial/)
*   [黄瓜:浏览器自动化](https://cucumber.io/docs/guides/browser-automation/)

## 硒

*   [web driver js:async/await](https://b.remarkabl.org/34tlEFy)
*   [用含硒的壁虎驱动](https://b.remarkabl.org/2eDO1Xp)
*   [BrowserStack:硒黄瓜 JS](https://www.browserstack.com/docs/automate/selenium/getting-started/nodejs/cucumber-js)
*   [硒-网络驱动程序文档](https://b.remarkabl.org/2etiPuZ)

[*本文最初于 2020 年 10 月 25 日在 remarkablemark.org 发表。*](https://b.remarkabl.org/34pXj3b)