# 在 React 和材质 UI 中创建确认对话框

> 原文：<https://javascript.plainenglish.io/creating-a-confirm-dialog-in-react-and-material-ui-3d7aaea1d799?source=collection_archive---------1----------------------->

![](img/2ce7bf33a9f1e7b581dd44ba499975e5.png)

在每个应用程序中都有你想删除某些东西的时候。所以像每个开发者一样，你添加一个按钮，当点击它时，删除资源。

无论是一篇博客文章、一个购物车中的商品，还是禁用一个帐户，您都希望防止不必要的按钮点击。

进入确认对话框。

有时候你确实想执行一个动作而不总是提示用户一个确认对话框，有时候总是提示用户会很烦人。

> 嘿，你想这么做吗？
> 
> 不，真的，你真的想这么做吗？

有时候，我会很烦，告诉自己，还有应用。是啊！我当然想做动作。不然我为什么会点击它？

然而，当涉及到删除敏感数据时，比如一篇博客文章，我建议添加一个确认对话框，这样用户就可以得到警告，如果他们不小心误点击了它，就可以退出。

在我们开始之前，让我们看看如何实现这是原生 JavaScript。

```
var shouldDelete = confirm('Do you really want to delete this awesome article?');if (shouldDelete) {
  deleteArticle();
}
```

这将提示一个默认的确认框，并提示用户“您真的想删除这篇很棒的文章吗？”

如果用户单击 Yes，那么它会将 shouldDelete 布尔值设置为 true，并运行 deleteArticle 函数。如果他们单击“否”或“取消”，将关闭对话框。

但是确认对话框的本地浏览器实现有点无聊，所以让我们做一个版本，看起来不错，有 React 和 Material UI。

让我们从创建一个可重用的组件开始。您可以在任何使用 React 和材质 UI 的应用程序中使用它。

```
import React from 'react';
import Button from '@material-ui/core/Button';
import Dialog from '@material-ui/core/Dialog';
import DialogActions from '@material-ui/core/DialogActions';
import DialogContent from '@material-ui/core/DialogContent';
import DialogTitle from '@material-ui/core/DialogTitle';const ConfirmDialog = (props) => {
  const { title, children, open, setOpen, onConfirm } = props;
  return (
    <Dialog
      open={open}
      onClose={() => setOpen(false)}
      aria-labelledby="confirm-dialog"
    >
      <DialogTitle id="confirm-dialog">{title}</DialogTitle>
      <DialogContent>{children}</DialogContent>
      <DialogActions>
        <Button
          variant="contained"
          onClick={() => setOpen(false)}
          color="secondary"
        >
          No
        </Button>
        <Button
          variant="contained"
          onClick={() => {
            setOpen(false);
            onConfirm();
          }}
          color="default"
        >
          Yes
        </Button>
      </DialogActions>
    </Dialog>
  );
};export default ConfirmDialog;
```

该组件将接受这些道具:

1.  标题—这是将显示为对话框标题的内容
2.  孩子—这是将在对话框内容中显示的内容。这可以是一个字符串，也可以是另一个更复杂的组件。
3.  打开—这是告诉对话框显示的内容。
4.  setOpen —这是一个状态函数，将对话框的状态设置为显示或关闭。
5.  onConfirm —这是用户单击“是”时的回调函数。

这只是一个基本的确认对话框，您可以修改它以满足您的需要，例如更改“是”或“否”按钮。

现在让我们看看如何在我们的应用程序中使用这个组件。

例如，假设我们有一个列出博客文章的表。我们希望当我们单击一个删除图标时运行一个函数，它将显示这个确认对话框，当我们单击“是”时，它将运行一个 deletePost 函数。

```
<div>
  <IconButton aria-label="delete" onClick={() =>     setConfirmOpen(true)}>
    <DeleteIcon />
  </IconButton>
  <ConfirmDialog
    title="Delete Post?"
    open={confirmOpen}
    setOpen={setConfirmOpen}
    onConfirm={deletePost}
  >
    Are you sure you want to delete this post?
  </ConfirmDialog>
</div>
```

在这个组件中，我们需要用属性 open、setOpen 和 onConfirm 实现 ConfirmDialog。Open 和 setOpen 通过使用 React 状态来控制，onConfirm 接受一个名为 deletePost 的函数，该函数调用一个 API 来删除这个特定的帖子。实现这些超出了本文的范围。我将让它来实现这些函数的实际功能。

你有它！创建一个可重复使用的确认对话框非常容易，它看起来比默认的本地浏览器对话框要好一百万倍。

## 进一步阅读

[](https://bit.cloud/blog/how-to-build-material-ui-components-with-bit-l3isiibs) [## 如何用 Bit 构建 React 材质 UI 组件

### Material UI 是一个流行的开源()UI 组件库，它将材质设计与 React 结合在一起。材料 UI 是…

比特云](https://bit.cloud/blog/how-to-build-material-ui-components-with-bit-l3isiibs) 

*更多内容请看*[***plain English . io***](https://plainenglish.io/)*。报名参加我们的* [***免费周报***](http://newsletter.plainenglish.io/) *。关注我们关于*[***Twitter***](https://twitter.com/inPlainEngHQ)[***LinkedIn***](https://www.linkedin.com/company/inplainenglish/)*[***YouTube***](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw)***，以及****[***不和***](https://discord.gg/GtDtUAvyhW) *对成长黑客感兴趣？检查* [***电路***](https://circuit.ooo/) ***。*****