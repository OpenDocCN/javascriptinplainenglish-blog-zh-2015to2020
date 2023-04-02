# 使用 React 路由器提示符编写自定义用户确认模式

> 原文：<https://javascript.plainenglish.io/creating-a-custom-userconfirmation-modal-with-the-react-router-prompt-dc1faa5a65a0?source=collection_archive---------0----------------------->

![](img/894e73fd91479037e9521e673366022c.png)

A person up in arms about wanting custom modals in their Prompt component

这是一个简短的演练，介绍如何使用 React-Router 的 v5 `Prompt` 组件将内置的用户确认窗口对象替换为您自己的自定义`UserConfirmation`模式。

还有另一篇真正的[好文章](https://medium.com/@michaelchan_13570/using-react-router-v4-prompt-with-custom-modal-component-ca839f5faf39)是关于阻止导航来阻止默认模态，以便我们可以呈现我们自己的定制组件。他们简单地提到了一种替代方法，包括利用`BrowserRouter`和`getUserConfirmation`道具，这正是这篇文章涵盖的解决方案！

# 短暂的幕后

不需要太多的细节，本质上你可以通过`[RouterContex](https://github.com/ReactTraining/react-router/blob/v5.0.0/packages/react-router/modules/RouterContext.js)t`、`[Router](https://github.com/ReactTraining/react-router/blob/v5.0.0/packages/react-router/modules/Router.js)`和`[BrowserRouter](https://github.com/ReactTraining/react-router/blob/v5.0.0/packages/react-router-dom/modules/BrowserRouter.js)`一路追踪`[Prompt](https://github.com/ReactTraining/react-router/blob/v5.0.0/packages/react-router/modules/Prompt.js)`组件。

奇迹开始了。具体在[第 11 行](https://github.com/ReactTraining/react-router/blob/v5.0.0/packages/react-router-dom/modules/BrowserRouter.js)创建历史道具的地方。`react-router`使用`[createBrowserHistory](https://github.com/ReactTraining/history/blob/v4.10.0/modules/createBrowserHistory.js)`创建我们的渲染历史道具，作为在任何浏览器中都可用的`window.history`的扩展。如果你查看`createBrowserHistory`，你也会看到我们确认窗口的最后一层，它来自于`[DomUtils](https://github.com/ReactTraining/history/blob/v4.10.0/modules/DOMUtils.js)`中定义的`getConfirmation`对`getUserConfirmation`的初始化。这就是默认模态被定义为窗口对象的地方。

```
export function getConfirmation(message, callback) {                            
   callback(window.confirm(message));
}
```

# 创建我们的自定义用户确认模式

因为我们选择了 window 对象并用我们自己的定制组件替换它，所以我们不仅需要呈现一个更好的模态，还需要包含取消和确认回调的所有功能。

记住`getUserConfirmation`是在`BrowserRouter`中初始化的，它决定了我们的确认模式显示什么。所以无论你在哪里用`BrowserRouter`初始化你的应用程序，我们都要把我们的定制组件传递到`getUserConfirmation` prop 中。

```
<BrowserRouter 
   getUserConfirmation={(message, callback) =>  
      UserConfirmation(message, callback)}> // our custom component
    <YourAwesomeApp />
</BrowserRouter>
```

你已经准备好了！您现在正在覆盖默认的窗口组件并呈现您自己的定制`UserConfirmation`(即使它可能还不存在)。

所以让我们实现它吧！

```
import React from "react";
import ReactDOM from "react-dom";
import { Dialog } from "evergreen-ui"const UserConfirmation = (message, callback) => {
   const container = document.createElement("div");
   container.setAttribute("custom-confirmation-navigation", "");
   document.body.appendChild(container);const closeModal = (callbackState) => {
      ReactDOM.unmountComponentAtNode(container);
      document.body.removeChild(container);
      callback(callbackState);
   }; ReactDOM.render(
      <Dialog
         cancelLabel="Cancel"
         confirmLabel="Confirm" 
         isShown={true}
         onCacel={() => closeModal(false)}
         onConfirm={() => closeModal(true)}         
         title="Warning"
      >
       {message}
      </Dialog>,
   container
  );
};export default UserConfirmation;
```

除了 UI 之外，我们还需要在自定义组件中做一些关键的事情。由于`BrowserRouter`和我们的定制组件是在我们的普通应用程序之上的一层呈现的，我们开始初始化一个容器，我们用它来直接呈现模态。然后，我们实际上定义了容器呈现的内容。

这是我们创建通过取消或确认操作关闭模式的功能的地方。在我们的`closeModal`函数中，我们卸载手动渲染的组件，要么将用户推到下一个位置，要么停留在`Prompt`被触发的位置。幸运的是，从`Prompt`传来的回调函数使得将用户发送到下一个位置或停留在页面上对我们来说非常简单。我们所需要传递的`callback`是一个布尔值，它告诉我们用户是否想继续前往他们的下一个位置。因此，当用户点击“取消”时，我们可以调用`callback(false)`，同样，当他们点击“确定”时，我们调用`callback(true)`。

就是这样！你完了。

# 但是我想要的不仅仅是一条信息和一个回电

这是大多数人会选择另一个解决方案的部分，因为他们希望在给我们的两个道具之外，对模型进行额外的定制。

可悲的是，这是一个合理的担忧。我们可以试着通过操纵我们的`message`道具来减轻这一点。我们的`Prompt`组件将只接受一个字符串进入`message`。因此，我们在这里可以做的是传入一个字符串化的 JSON 对象，其中包含我们希望对其进行额外控制的键值对，然后在以后解析它们。

```
<Prompt
   when={when}
   message={
      JSON.stringify(
         `{
           "confirmText": "Continue", 
           "messageText": "It looks like you might have some unsaved  
                       changes! Are you sure you want to continue?",
           "cancelText": "Do not Continue"
          }`   
      )
   }
/>
```

然后在我们的`UserConfirmation`模态中，我们需要创建一个`textObj`来引用我们定制的文本键，而不是直接使用`message`来显示文本。

```
const textObj = JSON.parse(message);<Dialog
   cancelLabel={textObj.cancelText}
   confirmLabel={textObj.confirmText}   
   isShown={true}
   onCacel={() => closeModal(false)}
   onConfirm={() => closeModal(true)}
   title="Warning"
>
   {textObj.messageText}
</Dialog>
```

现在，我们至少超越了一维模型，它只允许我们定制显示给用户的提示消息。类似地，您可以使用消息属性有条件地确定要呈现的内容。例如，如果我们没有传递一个`cancelText`，那么在我们的 Evergreen `[Dialog](https://evergreen.segment.com/components/dialog)`中，我们可以选择将`hasCancel`设置为 false，这样我们只呈现一个确认按钮，而不是关闭图标。

如果您有任何问题或反馈，请在下面留下您的评论！你可以在 [Medium](https://medium.com/@ivymarkwell) 和 [Github](https://github.com/ivymarkwell) 上关注我，看看我正在做的更多有趣的东西。特别感谢 [Joerg Baier](https://medium.com/u/f27d2add6165?source=post_page-----dc1faa5a65a0--------------------------------) 帮助我完成并理解了这个解决方案，以及我在 [Rabbet](https://rabbet.com/) 的许多其他同事给了我关于这篇文章的有用反馈。