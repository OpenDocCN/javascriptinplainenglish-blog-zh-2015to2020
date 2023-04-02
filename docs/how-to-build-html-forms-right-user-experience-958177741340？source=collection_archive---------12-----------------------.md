# 如何正确构建 HTML 表单:用户体验

> 原文：<https://javascript.plainenglish.io/how-to-build-html-forms-right-user-experience-958177741340?source=collection_archive---------12----------------------->

![](img/75bcc480413617fdd9e29ccd6f8a6d6f.png)

当您为 web 构建表单时，获得正确的语义、可访问性和样式需要做大量的工作。如果你能把这些都做好，你就做得很好了。然而，我们仍然可以做一些事情，让填写我们表格的人们生活得更好。

在这篇文章中，我们将看看一些关于 HTML 表单用户体验(UX)的注意事项。如果您想重温一下前面提到的步骤，可以看看本系列的其他文章。

*   [第一部分:语义](https://austingil.com/how-to-build-html-forms-right-semantics/)
*   [第二部分:无障碍](https://austingil.com/how-to-build-html-forms-right-accessibility/)
*   [第三部分:造型](https://austingil.com/build-html-forms-right-styling/)
*   第四部分:用户体验
*   [第 5 部分:安全性](https://austingil.com/how-to-build-html-forms-right-security/)

# 内容

## 要求最少的信息量

作为一个互联网用户，我可以凭经验说，在一个表单中输入过多的数据是令人讨厌的。所以如果你真的只需要一封电子邮件，考虑不要问名字，姓氏和电话号码。通过创建输入更少的表单，您将改善用户体验。一些研究甚至表明，更小的形式有更高的转化率。这对你来说是个胜利。此外，减少你收集的数据有可能减少你的隐私担忧，尽管这在很大程度上取决于数据。

## 保持简单

将你的创造力带入到表单设计中是很诱人的。然而，很容易走极端，让事情变得混乱。通过坚持使用标准输入类型的简单设计，你不仅在你的网站上，而且在互联网上创造了一个更有凝聚力的体验。这意味着用户不太可能被一些新奇的输入弄糊涂。坚持经典。请记住，像复选框(允许选择多个项目)这样的选择输入通常使用方框输入，而单选按钮(只允许选择一个项目)使用圆圈。

## 语义对 a11y 和 UX 都有好处

我在上一篇文章的[中更详细地讨论了语义，但是简短的说，选择正确的输入类型在许多层面上改善了体验:语义、可访问性和用户体验。人们已经习惯了网络上的输入方式，所以我们可以利用这一点，对同样的事情使用同样的输入。更不用说通过使用正确的输入，我们可以免费获得很多东西，比如键盘导航支持和验证。](https://austingil.com/how-to-build-html-forms-right-semantics/)

## 将国家选择器放在城市/州之前

对于任何向表单中添加区域设置的人来说，这是一个简单的规则。如果你要询问用户的国家，把它放在城市和州字段之前。原因是，通常城市和州会根据国家来填充。因此，如果您的国家选择默认为美国，而用户居住在墨西哥的瓦哈卡，他们将需要跳过城市和州字段，选择墨西哥的国家，然后返回并在列表更新后填写他们的城市和州。通过将国家放在第一位，可以保持表单的流畅，这对于使用键盘导航的用户来说特别好。

## 分页长表单

这与我的第一点有关，理想情况下，你没有太多的数据。然而，在某些情况下这是没有办法的。在这些情况下，对表单进行分页以便信息不会过多可能是有意义的。如果您选择对表单进行分页，我的最佳建议是向用户显示某种形式的 UI，显示他们在表单中的进度，以及删除分页并完整显示表单的选项。

# 一般功能

## 阻止浏览器刷新/导航

你是否曾经在填写一个很长的表单时，不小心刷新了页面，丢失了所有的工作？这是最糟糕的。幸运的是，浏览器为我们提供了`[beforeunload](https://developer.mozilla.org/en-US/docs/Web/API/Window/beforeunload_event)`事件，我们可以用它来通知用户他们将要丢失任何未保存的工作。

我们可以设置一个变量来跟踪表单是否有任何未保存的更改，并且我们可以将一个处理程序附加到`beforeunload`事件，如果有任何更改，它将阻止浏览器导航。

```
// You'll need some variable for tracking the status. We'll call it hasChanges here.window.addEventListener("beforeunload", (event) {
  if (!hasChanges) return; event.preventDefault();
  event.returnValue = "";
})form.addEventListener('change', () => {
  hasChanges = true;
});form.addEventListener('submit', () => {
  hasChanges = false;
})
```

这个片段的要点是我们正在跟踪一个叫做`hasChanges`的变量。如果当`beforeunload`事件触发时`hasChanges`是`false`，我们可以允许浏览器导航离开。如果`hasChanges`是`true`，浏览器将提示用户，让他们知道他们有未保存的更改，并询问他们是否要继续离开或留在页面上。最后，我们向表单添加适当的事件处理程序来更新`hasChanges`变量。

对于`hasChanges`变量，您的实现可能看起来略有不同。例如，如果您正在使用带有某种状态管理的 JavaScript 框架。如果你正在创建一个单页面应用程序，那么这个解决方案是不够的，因为`beforeunload`事件不会在单页面应用程序导航中触发。有关这方面的更多细节，请查看我的文章“[如何在 Vue](https://austingil.com/prevent-browser-refresh-url-changes-route-navigation-vue/) 中防止浏览器刷新、URL 更改或路线导航”。

## 存储未保存的更改

与上一点一样，有时我们会意外地丢失长表单上的所有工作。幸运的是，通过利用像`[sessionStorage](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage)`这样的浏览器功能，我们可以避免给用户带来这种痛苦。例如，我们想在任何时候发生变更事件时将所有数据存储在一个表单中。我们可以使用`[FormData](https://developer.mozilla.org/en-US/docs/Web/API/FormData)`来捕获表单及其所有当前值，然后将数据作为一个`JSON`字符串存储在`sessionStorage`中。

```
const form = document.querySelector('form')form.addEventListener('change', event => {
  const formData = new FormData(form);
  sessionStorage.setItem('your-identifier', JSON.stringify(formData));
});
```

随着数据的保存，用户可以刷新他们想要的，数据不会丢失。下一步是检查页面加载上的`localStorage`,看看我们是否有任何先前保存的数据来预填充表单。如果这样做，我们可以将字符串解析成一个对象，然后遍历每个键/值对，并将保存的数据添加到各自的输入中。对于不同的输入类型略有不同。

```
const previouslySavedData = sessionStorage.getItem('form-data');if (previouslySavedData) {
  const inputValues = JSON.parse(savedData); for(const [name, value] of Object.entries(inputValues)) {
    const input = form.querySelector(`input[name=${name}]`);
    switch(input.type) {
      case 'checkbox':
        input.checked = !!value;
        break;
      // other input type logic
      default:
        input.value = value;
    }
  }
}
```

最后要做的事情是确保一旦提交了表单，我们就清除所有以前保存的数据。这也是我们用`sessionStorage`代替`localStorage`的部分原因。我们希望我们保存的数据是暂时的。

```
form.addEventListener('submit', () => {
  sessionStorage.removeItem('form-data');
});
```

关于这个特性，最后要说的是，它并不适用于所有数据。任何私有或敏感数据都应该被排除在任何`localStorage`持久性之外。一些简单的输入类型不起作用。例如，没有办法持久保存文件输入。然而，理解了这些注意事项之后，它可以成为添加到几乎任何表单中的一个很好的特性。尤其是任何更长的形式。

## 不要阻止复制/粘贴

最近经历的最烦的一件事就是在国税局网站上。他们问我银行账号和银行代码。这些不是短数字，我们说的是 15 个字符。在大多数网站上，这没有问题，我从我的银行网站上复制数字并粘贴到输入栏中。然而，在国税局的网站上，他们选择禁止粘贴到输入中，这意味着我必须手动填写每个号码的详细信息…两次。我不知道他们为什么这样做，但这对用户来说非常令人沮丧，实际上增加了出错的可能性。请不要这样做。

# 输入功能

## 输入模式

如果你以前没有听说过`[inputmode](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/inputmode)`，那么现在让我向你介绍一下。`inputmode`是一个 HTML 输入属性，让你告诉浏览器输入格式。这可能不会马上清楚，如果你在你的台式电脑上，你不会注意到它，但对于移动用户来说，这有很大的不同。通过选择不同的输入模式，浏览器将向用户呈现不同的虚拟键盘来输入他们的数据。

只需添加不同的输入模式，就可以大大改善移动用户填写表单的用户体验。例如，如果您需要像信用卡号这样的数字数据，您可以将`inputmode`设置为`numeric`。这使得用户更容易添加数字。电子邮件也是一样，`inputmode=email`。

`inputmode`的可用值有`none`、`text`、`tel`、`url`、`email`、`numeric`、`decimal`和`search`。更多例子，请查看[inputmodes.com](https://inputmodes.com/)(最好是在移动设备上)。

## 自动完成

与`inputmode`一起，`[autocomplete](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/autocomplete)`属性是一个内置特性，可以极大地改善表单的用户体验。许多许多网站使用表单向用户索取相同的信息:电子邮件、地址、电话、信用卡等。浏览器内置的一个非常好的特性是用户可以保存自己的信息，这样就可以跨不同的表单和站点自动完成。`autocomplete`让我们深入了解这一点。

autocomplete 属性对任何文本或数字输入以及`<textarea>`、`<select>`和`<form>`元素都有效。我在这里列出了许多可用值，但一些突出的是`current-password`、`one-time-code`、`street-address`、`cc-number`(以及各种其他信用卡选项)和`tel`。

提供这些选项可以为许多用户带来更好的体验，不要担心这是一个安全问题，因为信息只存在于用户机器上，他们必须允许他们的浏览器实现它。

## 自（动）调焦装置

我要提到的最后一个内置属性是`[autofocus](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/Input#htmlattrdefautofocus)`。通过将它添加到输入中，浏览器会将焦点放在输入、选择或文本区域上(Chrome 也支持在`<button>`、`<a>`和带有`tabindex`的元素上使用它)。如果当前页面的主要目的是填写表单，这将非常有用。例如，如果你打开[duckduckgo.com](https://duckduckgo.com/)，你会注意到搜索输入已经被聚焦。这不是默认行为，但是他们已经添加了。挺好的。

然而，这里有一点需要注意。并不是每种形式都适合`autofocus`。将焦点放在一个元素上会滚动到该元素。因此，如果页面上有其他内容，我们可以滚动浏览所有内容。对于依赖屏幕阅读器等辅助技术的用户来说，这是一种特别不和谐的体验。只有当该功能确实改善了所有用户的体验时，请使用该功能。

## 自动扩展文本区域

一个非常小的功能，但我欣赏的是一个自动扩展以匹配内容的`textarea`。这样你就不必处理巨大的文本区域，或者那些太小的需要滚动条才能移动的区域。这可能不是每个用例都适合的特性，但是它确实可以为一些表单增添一些光彩。这里有一个简单的实现。

```
textarea.addEventListener('input', () => {
  textarea.style.height = "";
  textarea.style.height = Math.min(textarea.scrollHeight, 300) + "px";
});
```

我称之为幼稚的实现，因为根据我的经验，由于不同的站点对文本区域应用不同的 CSS 规则，很难得到一个通用的解决方案。有时会受到`padding`或`border-width`的影响，有时是因为`box-sizing`属性不同。在任何情况下，你都可以以此为起点，当然你也可以使用一个[库](https://www.jacklmoore.com/autosize/)。

## 数字输入时禁用滚动事件

如果你不熟悉，有一个关于数字输入的浏览器特性，允许你使用鼠标滚轮增加或减少数值。如果您需要快速更改值并且不想键入，这是一个很好的特性。然而，这个特性也可能导致错误，因为在需要滚动的长页面上，当用户想要向下滚动页面时，他们有时会意外地减少输入。有一个足够简单的解决方案:

```
<input type="number" onwheel="return false;" />
```

通过添加这个`[onwheel](https://developer.mozilla.org/en-US/docs/Web/API/Element/wheel_event)`事件处理程序，我们基本上是告诉浏览器忽略那个事件(尽管它仍然会触发任何附加的`wheel`事件)。因此，如果我们处理的是数字，比如地址、邮政编码、电话号码、社会保险号、信用卡号，或者其他明显不需要增加或减少的数字，我们可以使用这个方便的代码片段。然而，在这些情况下，我可能会建议使用一个`text`输入来代替，根本不用担心这个问题。

# 确认

验证是指获取一些表单数据，并确保它与您正在寻找的格式相匹配。例如，如果您希望某人在表单中提交一封电子邮件，您需要验证它是否包含一个`@`符号。有许多不同类型的验证和方法。一些验证发生在客户端，另一些发生在服务器端。我们将了解一些“该做”和“不该做”的事情。

## 延迟验证以模糊或提交事件

有了 HTML5，向表单添加一些客户端验证变得非常容易。您也可以决定用一些 JavaScript 来增强它，但是何时选择验证输入很重要。

假设您有一个函数，它接受一个输入 DOM 节点，检查它的`[ValidityState](https://developer.mozilla.org/en-US/docs/Web/API/ValidityState)`，并切换一个类是否有效:

```
function validate(input) {
  if (input.validity.valid) {
    input.classList.remove('invalid')
  } else {
    input.classList.add('invalid')
  }
}
```

您必须选择何时运行此功能。可以是用户点击输入、按键、离开输入或提交表单的任何时候。我的建议是为`[blur](https://developer.mozilla.org/en-US/docs/Web/API/Element/blur_event)`事件(当输入失去焦点时)或者表单的`[submit](https://developer.mozilla.org/en-US/docs/Web/API/HTMLFormElement/submit_event)`事件保留验证事件。在初始焦点上进行验证似乎不合适，在按键上进行验证可能会很烦人。就像你还没说完评论就有人想纠正你。

在大多数情况下，我喜欢将验证逻辑绑定到提交事件。我认为它简化了事情，并在我还需要一些服务器端验证逻辑的情况下保持了一个更有凝聚力的体验。也就是说，`blur`事件也是验证事情的一个非常方便的地方。

## 不要隐藏验证标准

另一个有用但不明显的技巧是预先清楚地告诉用户什么使输入有效或无效。通过共享该信息，他们已经知道他们的新密码需要 8 个字符长，包含大小写字母，并包括特殊字符。他们不必经历尝试一个密码，然后被告知需要选择另一个密码的步骤。

我建议用两种方式来实现这一点。如果是基本格式，也许可以使用一个`placeholder`属性。对于更复杂的情况，我建议将需求以纯文本的形式直接放在输入的下面，并在输入中包含一个`aria-labelledby`属性，这样这些需求也可以传递给辅助技术用户。

## 立即发回所有服务器验证错误

用户在填写表单时的另一个非常恼人的经历是，由于某些数据无效，不得不多次重新提交同一个表单。这可能是因为服务器一次只验证一个字段并立即返回错误，或者因为输入有多个验证条件，但服务器在遇到第一个验证条件时就返回验证错误，而不是捕获每个错误。

举个例子，假设我有一个注册表单，需要我的电子邮件和密码，密码至少有八个字符，至少一个字母，至少一个数字。最坏的情况是，如果我不知道任何更好的，我可能需要多次重新提交表单。

*   错误，因为我没有包括电子邮件
*   错误，因为我的密码太短
*   错误，因为我的密码需要包含字母
*   错误，因为我的密码需要包含数字
*   成功！

作为编写表单的开发人员，我们并不总是能够控制后端逻辑，但是如果我们控制了，我们应该尝试将所有错误作为一条消息返回:“第一个输入必须是一封电子邮件。密码必须是 8 个字符。只能包含字母和数字。密码必须包含 1 个字母和 1 个数字。或者类似的东西。然后用户可以一次修复所有错误并重新提交。

# 服从

## 用 JavaScript 提交

不管你对 JavaScript 在我们生活中的爆炸式增长有什么感觉，不可否认的是，它是一个让用户体验更好的有用工具。表单就是一个很好的例子。我们可以使用 JavaScript 来避免页面重载，而不是等待浏览器提交表单。

为此，我们向`[submit](https://developer.mozilla.org/en-US/docs/Web/API/HTMLFormElement/submit_event)`事件添加一个事件监听器，通过将表单(`event.target`)传递到`[FormData](https://developer.mozilla.org/en-US/docs/Web/API/FormData)`来捕获表单的输入值，并使用`[fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)`和`[URLSearchParams](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams)`的组合将数据发送到目标 URL ( `form.action`)。

```
function submitForm(event) {
  const form = event.target
  const formData = new FormData(form)

  fetch(form.action, {
    method: form.method,
    body: new URLSearchParams(formData)
  })

  event.preventDefault()
}document.querySelector('form').addEventListener('submit', submitForm)
```

处理程序末尾的`event.preventDefault()`对于来说很重要，这样浏览器就不会执行默认的通过 HTTP 请求提交事件的行为。这将导致页面重新加载，并不是一个好的体验。这里的一个关键点是，我们把这个方法放在最后，以防万一我们在处理程序中的某个更高的地方有一个异常，我们的表单仍然会退回到 HTTP 请求，表单仍然会被提交。

## 包括状态指示器

这一技巧与前一技巧紧密相连。如果我们要用 JavaScript 提交表单，我们需要更新用户的提交状态。例如，当用户点击提交按钮时，应该有某种指示(理想情况下是可视的和非可视的)表明请求已经发送。事实上，我们可以考虑 4 种状态:

*   在发送请求之前(这里可能不需要什么特别的东西)
*   请求待定。
*   收到成功响应。
*   收到失败的响应。

对于我来说，有太多的可能性告诉你在你的情况下你到底需要什么，但关键是你要记得考虑所有这些。不要让用户怀疑发送的请求是否有错误。(这是让他们向提交按钮发送垃圾邮件最快方法)。不要假设每个请求都会成功。告诉他们有一个错误，如果可能的话，如何解决它。并在他们的请求成功时给他们一些确认。

## 滚动到错误

如果你的表单出错了，最好让用户知道到底哪里出错了(如上所述)以及**在哪里**。尤其是在长滚动页面上，用户可能会尝试提交一个有某种错误的表单，即使您将输入内容涂成红色并添加一些验证错误消息，他们也可能看不到，因为它不在他们所在的屏幕上。

再一次，JavaScript 可以通过搜索表单中的第一个无效 input 元素，并关注它来帮助我们。浏览器会自动滚动到任何获得焦点的元素，因此只需很少的代码，就可以提供更好的体验。

```
function focusInvalidInputs(event) => {
  const invalidInput = event.target.querySelector(':invalid')
  invalidInput.focus() event.preventDefault()
}document.querySelector('form').addEventListener('submit', focusInvalidInputs)
```

我能给你的就这些了。用户体验是一个非常主观的事情，这个列表并不是完全完整的，但是我希望它能为你提供一些改进表单的概念和模式。

如果你喜欢这篇文章，如果你能[分享它](https://twitter.com/intent/tweet?url=https%3A%2F%2Fstegosource.com%2Fbuild-html-forms-right-styling%2F&text=Check%20out%20this%20article%20by%20%40Stegosource%20about%20design%20patterns%2C%20common%20gotchas%2C%20and%20CSS%20tips%20and%20snippets%20for%20building%20HTML%20forms.)，这对我真的意义重大。如果这是你想更经常看到的东西，你也应该[订阅我的时事通讯](https://austingil.com/newsletter/)和[在 Twitter 上关注我](https://twitter.com/stegosource)。