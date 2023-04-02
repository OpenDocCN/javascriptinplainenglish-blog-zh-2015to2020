# 创建自定义笑话匹配器，像专业人士一样进行测试

> 原文：<https://javascript.plainenglish.io/create-custom-jest-matchers-to-test-like-a-pro-3ee5f3d7a702?source=collection_archive---------1----------------------->

![](img/1d12625677eb112534ad0f6f30815f9f.png)

Jest 是 JS 项目的行业标准测试框架。它是由脸书开发的，用来测试他们的代码，并且是开源的。在本文中，我将教你创建自定义的笑话匹配器来增强你的测试技能。

# 什么是火柴人？

匹配器(或断言)是用于检查特定条件的函数。最常见的是，它比较两个值。以下是 Jest 中可用的一些基本匹配:

# 为什么我们需要定制匹配器？

虽然 Jest 是非常强大的开箱即用，但总有一些特定于项目的东西可以添加。它应该通过为常见用例创建自定义 Jest 匹配器来提高可读性并减少代码量。

在我的一个项目中，我必须测试某个值是否是格式正确的 ISO 日期。这可以用正则表达式来完成，但是我不得不测试它很多次，把这个逻辑移到一个定制的 Jest matcher 是值得的。

# 添加安装文件

在我们添加匹配器本身之前，为 Jest 添加一个设置文件是很重要的。设置文件是一个用来设置环境和做一些事情的文件，比如添加自定义匹配器、启用模拟和配置 jest。在项目根目录下创建一个名为`setupJest.js`的文件，这将是我们的设置文件。现在你需要将这一行添加到你的 Jest 配置中(在`package.json`或`jest.config.js`):

```
"setupFilesAfterEnv": ["./setupJest.js"]
```

**React Native/Expo 用户注意事项**:如果您正在使用 jest-expo 预设，这将不起作用。问题是预置覆盖了`setupFilesAfterEnv`设置。要避免这种情况，请像这样编写您的配置:

现在，您覆盖预设的`setupFilesAfterEnv`设置并添加您自己的文件。

# 编写自定义笑话匹配器

在这个例子中，我将编写一个匹配器来检查一个字符串是否是格式正确的 ISO 日期。这是使用`expect.extend`功能完成的。它接受一个对象，该对象的键是匹配器名称，值是执行实际匹配的函数。下面是进入`setupJest.js`的代码:

可以看到`toBeISODate`是一个接受单个值`received`的函数。您可以接受更多的参数，这些参数将被直接传递给匹配器(稍后会详细介绍)。在第 4-6 行，我们对照 regexp 检查`received`，在第 14-15 行，我们检查 JS 是否能正确解析`received`。

匹配器必须返回一个具有两个属性的对象:`pass`和`message`。`pass`很简单:如果检查通过则为真，否则为假。`message`稍微有点混乱。当`pass=false`时，`message`是`expect(something).toBeISODate()`失败时显示的错误。当`pass=true`时，`message`是当`expect(something).not.toBeISODate()`失败时将显示的错误。它永远不会显示测试是否通过。现在你可以这样使用这个匹配器:

```
const timestamp = get_timestamp_from_somewhere_unreliable(); expect(timestamp).toBeISODate();
```

# 接受更多的论点

`toBeISODate()`匹配器不接受任何额外的参数，也不需要接受。但是，假设我们想写一个匹配器来检查一个数是否是其他数的幂。下面是这样一个匹配器的代码，`toBePowerOf()`:

它接受另一个参数，`power`。您可以像这样在代码中使用这个匹配器:

```
expect(8).toBePowerOf(2); // passes 
expect(10).toBePowerOf(3); //fails expect('abacaba').toBePowerOf(); // throws
```

感谢您阅读本文，希望对您有所帮助。查看我关于 JavaScript 的其他文章:

[](https://medium.com/javascript-in-plain-english/improve-your-redux-skills-by-writing-custom-middleware-32a70b9dfb25) [## 通过编写定制中间件来提高您的 Redux 技能

### 在本文中，我将教您编写定制的中间件来扩展 Redux 的功能，并深入了解

medium.com](https://medium.com/javascript-in-plain-english/improve-your-redux-skills-by-writing-custom-middleware-32a70b9dfb25) [](https://medium.com/javascript-in-plain-english/react-usememo-and-when-you-should-use-it-e69a106bbb02) [## React.useMemo 以及何时应该使用它

### 尽管 React 经过了很好的优化，开箱即用，但了解它提供的制作工具非常重要…

medium.com](https://medium.com/javascript-in-plain-english/react-usememo-and-when-you-should-use-it-e69a106bbb02) [](https://medium.com/javascript-in-plain-english/10-javascript-interview-questions-for-2020-697b40de9480) [## 2020 年 10 个 JavaScript 面试问题

### 随着对 JS 开发人员需求的增长，你必须做好准备。这里有 10 个问题可以帮助你…

medium.com](https://medium.com/javascript-in-plain-english/10-javascript-interview-questions-for-2020-697b40de9480) [![](img/446049aa060bbaea15a64e1a907b1030.png)](http://eepurl.com/gYiA29)