# 如何在 React 中使用物化 CSS 模式

> 原文：<https://javascript.plainenglish.io/how-to-use-materialize-css-modal-in-react-53f9c85ba40d?source=collection_archive---------0----------------------->

![](img/e335f3d10cada031ed67fb777e5be9db.png)

[点击此处](https://reactize.herokuapp.com/modal)查看我们今天将在 Materialize CSS 和 React 的帮助下构建什么。

[Materialize](https://materializecss.com/) 是一个基于 Google Material 设计的 CSS 框架。这个框架的三个主要特点是，它加快了开发过程，非常容易使用，并且主要关注用户体验。

React 是一个声明式的基于组件的 javascript 库。它在虚拟 DOM 上工作，只渲染被改变的部分。

因为 React 是基于组件的，所以它可以与物化 CSS 一起使用，以便创建一些好的 UI 组件。但随之而来的问题是，Materialize 提供的 Javascript 组件操纵 DOM，很难将这些第三方 DOM 库与 React 集成。

因此，为了集成这些 DOM 库，必须使用 React 的一部分`ref`。人们可以从这里读到参考文献。

根据 React 文档，可以在以下情况下使用参考

*   触发命令式动画
*   与第三方 DOM 库集成

# **NPM**

1.  借助`npx`创建一个 react app。

```
npx create-react-app demo
```

2.安装物化 CSS npm 模块。

```
npm install materialize-css@next
```

# **项目结构**

我们需要遵循的项目结构如下-

```
public/
|--index.html
src/
|--Modal.js
|--index.js
--package.json
```

安装完所有的依赖项(只有一个😛)并检查了项目结构之后，我们可以将注意力集中在本文的主要部分，即如何创建一个物化 CSS 组件。

# **Modal.js**

正如我们前面讨论的使用 refs 的地方一样，在我们的例子中，我们使用 refs 为这个类名为“modal”的特定 div 标签创建一个引用。创建引用后，我们可以在生命周期方法中使用它— `componentDidMount()`。

```
<div
    ref={Modal => {
        this.Modal = Modal;
    }}
    className="modal">
</div>
```

代码的另一部分几乎类似于物化文档中给出的内容

```
import React, { Component } from "react";
import M from "materialize-css";
import "materialize-css/dist/css/materialize.min.css";

class Modal extends Component {
  componentDidMount() {
    const options = {
      onOpenStart: () => {
        console.log("Open Start");
      },
      onOpenEnd: () => {
        console.log("Open End");
      },
      onCloseStart: () => {
        console.log("Close Start");
      },
      onCloseEnd: () => {
        console.log("Close End");
      },
      inDuration: 250,
      outDuration: 250,
      opacity: 0.5,
      dismissible: false,
      startingTop: "4%",
      endingTop: "10%"
    };
    M.Modal.init(this.Modal, options);
  }

  render() {
    return (
      <>
        <a
          className="waves-effect waves-light btn modal-trigger"
          data-target="modal1"
        >
          Modal
        </a>

        <div
          ref={Modal => {
            this.Modal = Modal;
          }}
          id="modal1"
          className="modal"
        >
          <div className="modal-content">
            <h4>Modal Header</h4>
            <p>A bunch of text</p>
          </div>
          <div class="modal-footer">
            <a className="modal-close waves-effect waves-red btn-flat">
              Disagree
            </a>
            <a className="modal-close waves-effect waves-green btn-flat">
              Agree
            </a>
          </div>
        </div>
      </>
    );
  }
}

export default Modal;
```

我们在`ref`的帮助下创建了一个引用，然后在`componentDidMount()`方法中使用它。

在普通的 JavaScript 中，初始化部分是这样完成的—

```
document.addEventListener('DOMContentLoaded', function() {
    var elems = document.querySelector('.modal');
    var instances = M.Modal.init(elems);
  });
```

但是在 React 中，它是这样完成的—

```
componentDidMount() {
    M.Modal.init(this.Modal);
}
```

因此，主要概念是关于基准电压源的，其他元件也可以遵循同样的概念。

你可以在这个网站上查看其他组件— [Reactize](https://reactize.herokuapp.com)

https://github.com/GermaVinsmoke/Reactize

这是我第一次在媒体上写文章，所以任何形式的反馈都将受到感谢😅。如果您想知道如何将物化 CSS 与 React 和 Redux 一起用于表单组件，那么您可以等待新的文章😛

*更多内容尽在*[plain English . io](http://plainenglish.io/)