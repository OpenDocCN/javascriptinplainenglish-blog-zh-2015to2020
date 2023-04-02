# 几乎所有 JavaScript 字符串函数的有用示例

> 原文：<https://javascript.plainenglish.io/useful-examples-of-almost-all-javascript-string-functions-4131f3f990f2?source=collection_archive---------4----------------------->

![](img/1fbf9924cc80ecc9690b616c47c6d826.png)

Photo by [Tara Evans](https://unsplash.com/@taradee?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/string?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

如果您对 JavaScript 稍有了解，那么您很可能以前使用过 JavaScript 字符串函数。可能是`concat()`或`trim()`，但不太可能你已经使用了全部或大部分。

让我们来看几个例子，它们可能会让您在使用 JavaScript 字符串时更加轻松。

这篇文章的动机是我非常喜欢吉拉/ BitBucket 中的一个特性，当创建一个新的 git 分支时，这个特性会自动将吉拉票号和描述连接起来作为分支名称。

遗憾的是，这个功能在 Azure DevOps 中并不存在。让我们看看我们能否用一些 JavaScript 重新创建它！

让我们从一个简单的字符串开始，它将包含我们的票号和描述。

```
const userFeature = ` 12345: Create shipping component with 'Use this address' and 'Add new address...' buttons `;
```

# 1.)修剪()

因此，在`userFeature`字符串中，您可能注意到的第一件事是在数字`` 12345`之前和最后一个单词`buttons ``之后有一个空格。

在我们的例子中，我们想要删除这两个空格，幸好这正是`trim()`操作将为我们完成的。

**trimStart()** *—* 如果您只想修剪从`` 12345`到``12345`的字符串的开头，可以使用这个选项

**trimEnd()** *—* 如果您只想修剪从`buttons ``到`buttons``的线的末端，可以使用这个选项

```
const userFeature = ` 12345: Create shipping component with 'Use this address' and 'Add new address...' buttons `;const trimmedUserFeature = userFeature.trim();console.log(`Result: ${trimmedUserFeature}`);
//Result: 12345: Create shipping component with 'Use this address' and 'Add new address...' buttons
```

# 2.)替换()

创建 git 分支时，大多数特殊字符和空格都是无效的。这是`replace()`功能的一个很好的用途。

第一个`replace()`将用一个`-`替换所有空白空间。

第二个`replace()`将用空字符串替换字符串中不是字符 a-z、A-Z、0–9 或`-`的任何内容。

***注意* :** *您可以使用一个字符串来表示* `*replace()*` *，但是我建议不要这样做，因为它只会替换第一个***事件。正则表达式将替换* ***所有*** *出现的地方。**

```
*const userFeature = ` 12345: Create shipping component with 'Use this address' and 'Add new address...' buttons `;const formattedUserFeature = userFeature
    .trim()
    .replace(/ /g, '-')
    .replace(/[^A-Za-z0-9-]+/g, '');console.log(`Result: ${formattedUserFeature}`);
//Result: 12345-Create-shipping-component-with-Use-this-address-and-Add-new-address-buttons*
```

# *3.)toLowerCase() & toUpperCase()*

*继续完善我们的字符串，我们可以利用`toLowerCase()`或`toUpperCase()`将我们的字符串格式化为全部小写或全部大写字母。*

*我将使用`toLowerCase()`来遵循 git 分支的命名约定。*

```
*const userFeature = ` 12345: Create shipping component with 'Use this address' and 'Add new address...' buttons `;const formattedUserFeature = userFeature
    .trim()
    .replace(/ /g, '-')
    .replace(/[^A-Za-z0-9-]+/g, '')
    .toLowerCase();console.log(`Result: ${formattedUserFeature}`);
//Result: 12345-create-shipping-component-with-use-this-address-and-add-new-address-buttons*
```

# *4.)concat()*

*如果您想使用同样的想法来创建一个文件而不是 git 分支呢？我们可以用`concat()`通过添加适当的文件扩展名作为参数来实现。*

****注意* :** `*concat()*` *对性能有负面影响，所以最好用字符串模板或简单的赋值运算符(* `*+*` *)* 添加 `*.js*`*

```
**const userFeature = ` 12345: Create shipping component with 'Use this address' and 'Add new address...' buttons `;const formattedUserFeature = userFeature
    .trim()
    .replace(/ /g, '-')
    .replace(/[^A-Za-z0-9-]+/g, '')
    .toLowerCase()
    .concat('.js');console.log(`Result: ${formattedUserFeature}`);
//Result: 12345-create-shipping-component-with-use-this-address-and-add-new-address-buttons.js**
```

# **5.)切片()**

**现在我们已经对`trim()`、`replace()`、`toLowerCase()`和`concat()`进行了上述修改，以制作`formattedUserFeature`字符串，让我们将该字符串用于以下操作。**

**假设您已经使用上面的方法创建了多个 git 分支(或文件),并且您想要获得所有的票号。假设票号都是五位数，您可以添加`0`的`beginIndex`和`5`的`endIndex`。**

```
**const formattedUserFeature = `12345-create-shipping-component-with-use-this-address-and-add-new-address-buttons.js`;console.log(`Result: ${formattedUserFeature.slice(0, 5)}`);
//Result: 12345**
```

# **6.)startsWith() / endsWith()**

**`startsWith()`和`endsWith()`都将搜索返回布尔值的字符串的开头或结尾。**

**如果您试图确定所有的`.js`或`.ts`文件，这可能会很有用。您也可以使用它来确定所有以`123`等开头的文件或分支。**

```
**const formattedUserFeature = `12345-create-shipping-component-with-use-this-address-and-add-new-address-buttons.js`;console.log(`Result: ${formattedUserFeature.startsWith('10')}`);
//Result: falseconsole.log(`Result: ${formattedUserFeature.endsWith('.js')}`);
//Result: true**
```

# **7.)包括()**

**如果你想确定一个单词(一个`string`)是否存在于另一个字符串中，你可以使用`includes()`。**

**在我们的例子中，我们可能希望看到所有与`shipping`相关的文件或分支，因此我们可以将其作为参数传递给`includes()`，后者将返回一个布尔值。**

```
**const formattedUserFeature = `12345-create-shipping-component-with-use-this-address-and-add-new-address-buttons.js`;console.log(`Result: ${formattedUserFeature.includes('shipping')}`);
//Result: true**
```

# **附加说明**

**如果你喜欢这篇文章，并想执行这段代码，并与我在 Medium 上发布的未来文章保持同步，请查看这个 [GitHub 库](https://github.com/tengel92/Medium)。**

**要了解更多细节，我建议查看一下 Mozilla Developer Network 的 JavaScript 字符串。**

**你也可以简单地在本地或者在 Chrome DevTools 中利用下面的 GitHub 要点执行这段代码。**

## ****用简单英语写的 JavaScript****

**喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！****