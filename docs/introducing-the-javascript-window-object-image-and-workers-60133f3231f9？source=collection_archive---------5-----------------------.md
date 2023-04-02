# JavaScript 窗口对象简介—图像和工人

> 原文：<https://javascript.plainenglish.io/introducing-the-javascript-window-object-image-and-workers-60133f3231f9?source=collection_archive---------5----------------------->

![](img/2266ed23e99b05b74470989633dba315.png)

Photo by [Nathan Fertig](https://unsplash.com/@nathanfertig?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

`window`对象是一个全局对象，具有与当前 DOM 文档相关的属性，即浏览器选项卡中的内容。`window`对象的`document`属性拥有 DOM 文档以及相关的节点和方法，我们可以用它们来操作 DOM 节点并监听每个节点的事件。因为`window`对象是全局的，所以它在应用程序的每个部分都是可用的。当一个变量在没有`var`、`let`或`const`关键字的情况下被声明时，它们会被自动附加到`window`对象上，使它们在你的 web 应用程序的每个部分都可用。这仅适用于禁用严格模式的情况。如果它被启用，那么声明没有`var`、`let`或`const`的变量将被停止，因为让我们意外地声明全局变量不是一个好主意。`window`对象有许多属性。它们包括构造函数、值属性和方法。有一些方法可以操作当前的浏览器标签，比如打开和关闭新的弹出窗口等。

在选项卡式浏览器中，每个选项卡都有自己的`window`对象，因此`window`对象总是代表代码运行时当前打开的选项卡的状态。但是，有些属性仍然适用于浏览器的所有选项卡，如`resizeTo`方法以及`innerHeight`和`innerWidth`属性。

注意，我们不需要直接引用`window`对象来调用方法和对象属性。例如，如果我们想使用`window.Image`构造函数，我们可以只写`new Image()`。

# 构造器

对象有一些构造器属性。它们包括将 XML 和 HTML 字符串解析成 DOM 元素、创建新的 HTML DOM 元素的对象，以及创建新的 Web workers 的对象。

## DOMParser

`DOMParser`构造函数让我们创建一个 DOM 解析器对象，将 XML 或 HTML 字符串解析成 DOM 树对象。对于 HTML，我们还可以设置从 DOM 解析器返回的 DOM 节点对象的`innerHTML`或`outerHTML`元素，以添加到 DOM 树中。这些属性也可以用来获取对应于 DOM 子树的 HTML 片段。构造函数不接受任何参数。创建的对象有`parseFroMString`方法，根据给定的字符串将 HTML 或 XML 字符串解析成 DOM 树对象。它需要两个参数。第一个参数是包含我们想要解析的 XML 或 HTML 代码的字符串。该字符串必须是 HTML、XML、XHTML+XML 或 SVG 文档。第二个参数是一个字符串，它具有我们作为第一个参数传入的代码字符串的 MIME 类型。MIME 类型可以是下列类型之一:

*   `text/html`
*   `text/xml`
*   `application/xml`
*   `application/xhtml+xml`
*   `image/svg+xml`

我们可以在下面的代码中使用它:

```
const xmlString = `
<note>
 <to>John</to>
 <from>Jane</from>
 <heading>Greeting</heading>
 <body>How are you?</body>
</note>
`;
const parser = new DOMParser();
const xmlDoc = parser.parseFromString(xmlString, "application/xml");
console.log(xmlDoc);
```

当我们运行上面的代码时，我们获得了带有 DOM 节点的 DOM 对象。我们也可以在 HTML 中使用`parseFromString`方法，如下面的代码所示:

```
const htmlString = `
<div>
 <p>From: John</p>
 <p>To: Jane</p>
 <p>Greeting</p>
 <p>How are you?</p>
</div>
`;
const parser = new DOMParser();
const htmlDoc = parser.parseFromString(htmlString, "text/html");
console.log(htmlDoc);
for (const p of htmlDoc.body.children[0].children) {
  console.log(p.innerHTML);
}
```

当我们运行上面的代码时，我们用第一个`console.log`语句获得带有 DOM 节点的 DOM 对象，这使我们:

```
<html>
  <head></head>
  <body>
    <div>
      <p>From: John</p>
      <p>To: Jane</p>
      <p>Greeting</p>
      <p>How are you?</p>
    </div>
  </body>
</html>
```

当我们在上面示例代码的底部循环时，我们得到:

```
From: John
To: Jane
Greeting
How are you?
```

这是因为`htmlDoc.body.children[0].children`为我们获取了 DOM 树对象，该对象有一个`body`属性来为我们获取`body`元素和其中的所有内容。该元素有一个`children`对象，该对象将`div`元素作为类似 DOM 树数组的对象的第一个元素，然后我们获得该对象的`children`属性，该对象有`p`元素。有了它，我们可以遍历它们并得到元素。在上面的例子中，我们使用`innerHTML`属性访问了`p`标签中的内容。

同样，我们可以将`parseFromString`与 SVG 元素一起使用，因为 SVG 图形是从 XML 文件生成的矢量图形。因此，我们可以使用相同的方法来解析 SVG 字符串。

## 图像

`Image`构造函数创建一个新的 HTML `image`元素。这和使用`document.createElement('img')`完全一样，它也创建了一个 HTML 元素。`Image`构造函数有两个参数。第一个参数是以像素为单位的`img`元素的宽度，第二个参数是以像素为单位的图像元素的高度。例如，我们可以这样使用它:

```
const image = new Image(100, 200);
image.src = '[https://images.unsplash.com/photo-1572315831029-5d6f20e0035d?ixlib=rb-1.2.1&auto=format&fit=crop&w=750&q=80'](https://images.unsplash.com/photo-1572315831029-5d6f20e0035d?ixlib=rb-1.2.1&auto=format&fit=crop&w=750&q=80');
document.body.appendChild(image);
```

上面的代码将创建一个 100 像素宽、200 像素高的`img`元素。然后，创建的图像元素将被附加到 HTML 文档的`body`元素中。如果我们运行上面的代码，我们应该会看到一个图像，其尺寸由构造函数的参数给出。这相当于:

```
<img width="100" height="200" src="[https://images.unsplash.com/photo-1572315831029-5d6f20e0035d?ixlib=rb-1.2.1&amp;auto=format&amp;fit=crop&amp;w=750&amp;q=80](https://images.unsplash.com/photo-1572315831029-5d6f20e0035d?ixlib=rb-1.2.1&amp;auto=format&amp;fit=crop&amp;w=750&amp;q=80)">
```

这也是`Image`构造函数输出的内容。

## [计]选项

`Option`构造函数创建一个`HTMLOptionElement`，它是一个带有`option`标签的 HTML 元素。构造函数有 4 个参数。第一个参数是选项的文本。这是一个可选的字符串参数。如果未指定，则使用空字符串作为默认值。第二个参数是`option`元素的值，它对应于 HTML `option`元素的`value`属性的值。如果没有指定，那么文本的值将被用作`value`属性的值。第三个参数是 boolean，当第一次加载这个元素时，它设置`selected`属性值。如果没有指定，那么`false`就是使用的值。设置`true`的值不会将选项设置为选中(如果它尚未被选中)。第四个参数是一个布尔值，它设置了`selected`属性的值。默认为`false`，这意味着默认情况下它不被选中。如果省略该参数，即使第三个参数是`true`，也不会选择该选项。这个构造函数的所有参数都是可选的。

例如，我们可以通过为`select`元素添加 HTML 代码来使用它:

```
<select id='select'></select>
```

然后我们可以添加获取`select`元素的 JavaScript 代码，然后用`Option`构造函数创建`option`元素，然后将它们追加到`select`元素，如下面的代码所示:

```
const select = document.getElementById('select');
const options = ['one', 'two', 'three'];for (const o of options) {
  select.append(new Option(o, o, o === 'one', o === 'one'));
}
```

在上面的例子中，如果`option`的值和文本是`'one'`，我们将构造函数的第三和第四个参数设置为`true`，因此第一个选项将是默认选择的选项。如果我们运行上面的代码，我们应该得到一个下拉框，默认情况下第一个选项被选中。

我们还可以将`option`元素的不同值设置为不同的属性。例如，我们可以写:

```
const select = document.getElementById('select');
const options = ['one', 'two', 'three'];for (const o of options) {
  if (o === 'one') {
    select.append(new Option(o, o, true, false));
  } else if (o === 'two') {
    select.append(new Option(o, o, false, true));
  } else {
    select.append(new Option(o, o, false, false));
  }
}
```

如果我们运行上面的代码，我们会发现在页面加载时默认选择第二个选项，而不是第一个。

![](img/4db568cdce33fb418016c6a72f4bba6b.png)

Photo by [Roberto Nickson](https://unsplash.com/@rpnickson?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 工人

`Worker`构造函数让我们创建一个 Web Worker，让我们在 Web 应用程序中后台处理任务。它可以很容易地被创建，并且可以用来向它的创建者发送消息。创建一个 worker 就像调用`Worker`构造函数并指定一个脚本来运行 Worker 线程一样简单。只要新工人与原始工人具有相同的起源，工人就可以产生新工人。只要`XMLHttpRequest`上的`responseXML`和`channel`属性总是返回`null`，工人就可以使用`XMLHttpRequest`进行 HTTP 请求。

为了创建一个 worker，我们使用了接受 2 个参数的`Worker`构造函数。第一个是一个字符串，其中包含 worker 将运行的脚本的 URL。它必须与产生该工作线程的脚本位于相同的源或域中。第二个参数是可选参数，它是一个具有下列属性的对象。`type`属性是一个字符串，它指定了要创建的 worker 的类型。这些值可以是`classic`或`module`。默认值为`classic`。`credentials`属性是一个字符串，它指定使用 worker 的凭证的类型。可能的值有`omit`、`same-origin`或`include`。如果没有指定或者类型是`classic`，那么默认值是`omit`，这意味着不需要凭证。最后，`name`属性是一个字符串属性，它让我们指定代表工作者范围的`DedicatedWorkerGlobalScope`的标识名。这主要用于调试目的。

一个`Worker`对象抛出异常。如果文档不允许启动 workers，它会抛出一个`SecurityError`。如果 URL 无效或没有遵循同源策略，就会发生这种情况。如果工作脚本的 MIME 类型不正确，就会引发`NetworkError`。应该一直是`text/javasceipr`。当无法解析 Worker 的 URL 时，会引发一个`SyntaxError`。

例如，我们可以通过从一个脚本向一个工作者脚本发送消息来构造一个工作者对象。然后工作脚本可以向主脚本发回消息。在下面的例子中，我们将通过编写一些代码与工人一起制作一个简单的计算器。首先，我们为输入创建 HTML 代码，如下所示:

```
<!DOCTYPE html>
<html>
  <head>
    <title>Add Worker</title>
  </head>
  <body>
    <form>
      <div>
        <label for="number1">First Number</label>
        <input type="text" id="number1" value="0" />
      </div>
      <div>
        <label for="number2">Second Number</label>
        <input type="text" id="number2" value="0" />
      </div>
    </form>
    <p id="result">Result</p>
    <script src="main.js"></script>
  </body>
</html>
```

创建一个文件夹，并将上面的代码保存到一个 HTML 文件中。然后在同一个文件夹中，创建一个`main.js`文件，并放入以下代码:

```
const worker = new Worker("worker.js");
const first = document.getElementById("number1");
const second = document.getElementById("number2");
const result = document.getElementById("result");
first.onkeyup = () => {
  worker.postMessage([first.value, second.value]);
};second.onkeyup = () => {
  worker.postMessage([first.value, second.value]);
};worker.onmessage = e => {
  result.textContent = e.data;
};
```

上面的代码将从 HTML 文件中获取输入的值，然后每当第一个或第二个输入有内容输入时就发送消息。监听每个输入的`keyup`事件让我们实现这个目标。然后在同一个文件夹中，创建一个`worker.js`文件，然后放入以下代码:

```
onmessage = e => {
  console.log("Worker received message");
  const [first, second] = e.data;
  let sum = +first + +second;
  if (isNaN(sum)) {
    postMessage("Both inputs should be numbers");
  } else {
    let workerResult = `Result: ${sum} `;
    console.log("Send message back to main scriot");
    postMessage(workerResult);
  }
};
```

注意，`worker.js`文件有一个`onmessage`处理程序。该函数是必需的，并且应该具有该名称。参数`e`具有从`main.js`文件发送的消息。我们可以像收到信息一样收到它。所以`e`参数应该有从`main.js`发送的数组。然后我们可以计算这个函数中的和，然后将计算的结果发送回`main.js`，它用`postMessage`函数生成了这个 worker。在`main.js`文件中，我们有`worker.onmessage`处理程序来监听从这个文件发回的消息。在这个文件中，我们发回了结果，这就是我们得到的结果，我们用它来设置 ID 为`result`的元素的结果。

在本文中，我们仅仅触及了`window`对象的表面。我们只讨论了在各种情况下可能会派上用场的几个构造函数。我们使用 DOMParser 将 HTML 和 XML 字符串解析成 DOM 树对象。同样，我们使用 Image 构造函数创建一个`img`元素，使用`Option`构造函数创建一个`option`元素。最后，我们使用 Worker 构造函数创建一个 Web Worker，让我们在后台运行代码，并在一个脚本和 Worker 脚本之间发送消息，反之亦然。产生 worker 的脚本和 worker 脚本必须托管在同一个域中，这样外部脚本就不能在我们的应用程序中产生 worker 并向我们的应用程序发送包含恶意数据的消息。`window`对象比一些构造函数有更多的属性，我们将在本系列的后面部分研究这些属性。