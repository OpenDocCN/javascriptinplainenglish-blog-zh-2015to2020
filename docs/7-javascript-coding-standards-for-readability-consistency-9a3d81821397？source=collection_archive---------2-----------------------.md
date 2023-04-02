# 7 可读性和一致性的 JavaScript 编码标准

> 原文：<https://javascript.plainenglish.io/7-javascript-coding-standards-for-readability-consistency-9a3d81821397?source=collection_archive---------2----------------------->

## 提高代码质量的 JavaScript 编码标准

![](img/f38edb1eeefde644cf4faab960b558ca.png)

Photo by Kevin Ku on [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

编码标准可以在以下方面提供帮助:

*   保持代码的一致性
*   更容易阅读和理解
*   更易于维护
*   更容易重构

下面的编码标准是我自己的**意见**关于什么可以帮助以上几点，使用我在开发和审查其他开发者 JavaScript 时的经验。

# 在承诺上使用异步等待

尽可能选择一个来使用。如果你喜欢承诺，那也很好，只是要保持一致，因为这将有助于可读性。

我选择 async-await，因为我发现它比承诺更容易阅读。我问了我的团队，团队中的每个开发人员都说 async-await 也更容易阅读！

当您必须进行不止一次调用时，这变得更加明显，例如，一个 API 需要来自另一个 API 的数据。

*失败:*

```
const getData = url => {
 fetch(url)
 .then((response) => {
   // return Json from first call 
   return response.json()
  })
 .then((data) => {   
   // do stuff with data, call second fetch
   return fetch(url + "/" + data.someID)
 })
 .then((response) => { 
   // return Json from first call  
   return response.json(); 
 })
 .then((data) => {
  // do stuff with `data` from second fetch
 })
 .catch((error) => { 
  console.log('Requestfailed', error) 
 })
 .finally(() => {
   // runs everytime 
 });
}
```

*通过:*

```
const getData = async url => {
   try {
      // fetch first call and get data   
      let data = await fetch(url).json();// fetch second call and get data
      data = await fetch(url + "/" + data.someID).json();// do stuff with `data` from second fetch}
   catch(err) {
      console.log('Error: ', err)
   }
   finally { 
      // runs everytime  
   }
}
```

# 变量

## **#使用**`**const**`**&**`**let**`**超过** `**var**`

*失败:*

```
var myVar = 10;
var iNeverHaveToChange = "me";
```

*通过:*

```
let myVar = 10;
const iNeverHaveToChange = "me";
```

使用`const`有助于可读性，因为开发人员知道它不能被重新分配！

使用`let`而不是`var`更多是因为`var`会导致范围问题。这可能是太多进入这个职位，因为它可能值得一个职位本身。

## **#避免使用全局变量**

尽量减少全局变量的使用。全局变量是一个非常糟糕的主意。全局变量和函数的问题是它们会被其他脚本覆盖。

## **#命名变量**

尽量想出有意义的名字，不要太长。在编码中给事物命名实际上是最难的事情之一！

`let` 应该是**茶包**

`const`如果在一个文件的顶部使用大写蛇形。`MY_CONST`。如果不在文件顶部，使用`camelCase`

# API 调用

## **#选择一种方法并坚持下去**

我所说的方法是指下面的一种。

*   XMLHttpRequest
*   取得
*   Axios
*   jQuery

现在 Axios & Fetch 是首选方式。使用 Axios 而不是`fetch`的一个主要好处是它广泛的浏览器支持。即使像 IE11 这样的老浏览器也可以毫无问题地运行 Axios。虽然有多种选择，但这取决于什么对你更重要。

## **#使通话可重复使用**

而不是在应用程序中到处调用。为你的电话准备好模块。这使得它更加一致，也更容易重构。此外，如果 API 发生变化，您只需更改一次即可！

# 进口

## **#尽可能使用命名导入而非默认导入**

这使得一切都一致，你知道会发生什么。此外，编辑器不提供自动完成建议。

`default` imports 的问题是任何开发者都可以随意命名它。这可能会使重构变得棘手！

*失败:*

```
export default class Apple {// This should be apple :/
import banana from "apple.js"
```

*通过:*

```
export class Apple {import { Apple } from "apple.js"
```

## 尝试按字母顺序排列你的进口商品。

导入声明的排序列表使开发人员以后更容易阅读和找到必要的导入

*失败:*

```
import banana from "banana.js";
import Apple from "apple.js";
import { g, e } from "other.js";
import { lion, bear, dog, cat } from "animals";
```

*过关:*

```
import { bear, cat, dog, lion } from "animals";
import { e, g } from "other.js"; 
import Apple from "apple.js";
import banana from "banana.js";
```

Eslint 在这方面真的很有帮助，以下是默认规则。这可以归结为风格和什么是最适合你的。我推荐挑一个坚持下去。

```
{
    "sort-imports": ["error", {
        "ignoreCase": false,
        "ignoreDeclarationSort": false,
        "ignoreMemberSort": false,
        "memberSyntaxSortOrder": ["none", "all", "multiple", "single"],
        "allowSeparatedGroups": false
    }]
}
```

# Dom 操作

取决于你是否在使用一个框架，你可能不需要担心以下几点，因为框架会为你处理 dom 操作

## **#查询选择**

使用`querySelector`和`querySelectorAll`而不是`getElementById`和`getElementsByTagName`方法。

它们之间的区别是`getElementsByTagName`返回一个动态的 HTML 集合，所以如果你用相同的标签向 DOM 添加更多的元素，它们将被添加到集合中。

将返回一个普通的 HTML 集合。它不会更新，所以它更像是代码执行时的快照。

除非你需要动态的 HTML 集合，为了一致性，最好选择一个并在你的脚本中坚持使用它。混合使用只会看起来很乱！

*失败:*

```
let element = document.getElementById("#unique_id");let elements = document.getElementsByTagName(".element_class");
```

*通过:*

```
// Selects element which has the ID "#unique_id"
let element = document.querySelector("#unique_id");// Selects the first element which has the class "element_class"
let element = document.querySelector(".element_class");// Select all the element which have the class "element_class"
let element = document.querySelectorAll(".element_class");
```

## **#使用 CSS 类而不是添加样式**

这使得代码更容易阅读，可重用性更高，如下图所示。

*失败:*

```
const customerName = document.querySelector('#customerName');
const form = document.querySelector('form');form.onsubmit = (e) => {
  e.preventDefault();
}input.oninvalid = () => {
  customerName.style.borderColor = 'red';
  customerName.style.borderStyle = 'solid';
  customerName.style.borderWidth = '1px';
}
```

*通过:*

```
// JavaScript
const customerName = document.querySelector('#customerName');
const form = document.querySelector('form');form.onsubmit = (e) => {
  e.preventDefault();
}input.oninvalid = () => {
  customerName.className = 'invalid';
}// CSS
.invalid {
  border: 1px solid red;
}
```

# 功能

## **#尽可能使用 ES6 箭头功能**

箭头函数是书写函数表达式的一种更简洁的语法。它们是匿名的，改变了`this`在函数中的绑定方式。

*失败:*

```
var multiply = function(a, b) {
  return a* b;
};
```

*通过:*

```
const multiply = (a, b) => a * b;
```

## **#命名功能**

`functions`应该是`camelCase`。`myFunction`

*失败:*

```
const Multiply = (a, b) => { return a * b};
const MULTIPLY = (a, b) => { return a * b};
```

*通过:*

```
const multiply = (a, b) => { return a * b};
```

# 结论

任何语言的编码标准都有助于提高应用程序的可读性和可维护性。他们中的许多人归结为偏好，并重申以上是我自己的观点。要点是保持一致，因为它们确实有助于以干净的方式扩展应用程序。

我认为上面的列表中缺少的一件事是如何构建应用程序。我没有添加这一点，因为这可能取决于您的应用程序，如果您使用的是框架。

如果你在团队中工作，另一件困难的事情是执行编码标准。以下是一些有帮助的想法:

*   案头审查，逐行检查代码。
*   林挺或者某种代码分析器
*   创建一致的应用程序，每个开发人员都知道他们需要创建/更新什么。

下面是一些更简单的编码标准，可以帮助你写出干净的 JavaScript 代码！

[](https://medium.com/javascript-in-plain-english/19-simple-javascript-coding-standards-to-keep-your-code-clean-7422d6f9bc0) [## 保持代码整洁的 19 个简单 JavaScript 编码标准

### 提高代码质量的 JavaScript 编码标准

medium.com](https://medium.com/javascript-in-plain-english/19-simple-javascript-coding-standards-to-keep-your-code-clean-7422d6f9bc0) 

希望你喜欢阅读！

## **用简单英语写的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅获取更多类似内容解码，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**