# 材料用户界面—对话框

> 原文：<https://javascript.plainenglish.io/material-ui-dialogs-bd6279d4930c?source=collection_archive---------5----------------------->

![](img/baf6259411fe01b337070d9ce25cbc82.png)

Photo by [Ben White](https://unsplash.com/@benwhitephotography?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

材质 UI 是一个为 React 制作的材质设计库。

这是一组具有材质设计风格的 React 组件。

在这篇文章中，我们将看看如何添加带有材质 UI 的对话框。

# 对话

对话框用于让用户了解一些信息。

要添加一个，我们可以使用`Dialog`组件。

例如，我们可以写:

```
import React from "react";
import Button from "[@material](http://twitter.com/material)-ui/core/Button";
import Dialog from "[@material](http://twitter.com/material)-ui/core/Dialog";
import DialogTitle from "[@material](http://twitter.com/material)-ui/core/DialogTitle";
import List from "[@material](http://twitter.com/material)-ui/core/List";
import ListItem from "[@material](http://twitter.com/material)-ui/core/ListItem";export default function App() {
  const [open, setOpen] = React.useState(false); const handleClose = () => {
    setOpen(false);
  }; return (
    <div>
      <Button variant="outlined" color="primary" onClick={() => setOpen(true)}>
        Open simple dialog
      </Button>
      <Dialog onClose={handleClose} open={open}>
        <DialogTitle id="simple-dialog-title">dialog</DialogTitle>
        <List>
          <ListItem autoFocus button onClick={handleClose}>
            close
          </ListItem>
        </List>
      </Dialog>
    </div>
  );
}
```

添加包含某些项目的对话框。

我们添加了一个带有`ListItem`的`List`，它是一个按钮。

我们可以点击它来关闭带有`false`参数的`setOpen`函数对话框。

然后`open`道具控制是否打开。

`handleClose`让我们通过用`setOpen`将`open`设置为`false`来关闭对话框。

`variant`设置为`outlined`让我们添加一个轮廓。

# 警报

警报用于显示紧急消息。

要添加一个，我们可以使用我们以前拥有的组件，并添加`DialogContent`、`DialogContentText`和`DialogActions`组件来添加对话框内容。

例如，我们可以写:

```
import React from "react";
import Button from "[@material](http://twitter.com/material)-ui/core/Button";
import Dialog from "[@material](http://twitter.com/material)-ui/core/Dialog";
import DialogTitle from "[@material](http://twitter.com/material)-ui/core/DialogTitle";
import DialogContent from "[@material](http://twitter.com/material)-ui/core/DialogContent";
import DialogContentText from "[@material](http://twitter.com/material)-ui/core/DialogContentText";
import DialogActions from "[@material](http://twitter.com/material)-ui/core/DialogActions";export default function App() {
  const [open, setOpen] = React.useState(false); const handleClose = () => {
    setOpen(false);
  }; return (
    <div>
      <Button variant="outlined" color="primary" onClick={() => setOpen(true)}>
        Open simple dialog
      </Button>
      <Dialog open={open} onClose={handleClose}>
        <DialogTitle>Confirm</DialogTitle>
        <DialogContent>
          <DialogContentText>Are you sure?</DialogContentText>
        </DialogContent>
        <DialogActions>
          <Button onClick={handleClose} color="primary">
            no
          </Button>
          <Button onClick={handleClose} color="primary" autoFocus>
            yes
          </Button>
        </DialogActions>
      </Dialog>
    </div>
  );
}
```

来补充一切。

`DialogTitle`有标题。

`DialogContent`有内容。

`DialogContentText`是我们放置对话框文本的地方。

`DialogActions`有按钮让用户做一些事情。

# 过渡

我们可以在对话框打开时添加动画过渡效果。

例如，我们可以写:

```
import React from "react";
import Button from "[@material](http://twitter.com/material)-ui/core/Button";
import Dialog from "[@material](http://twitter.com/material)-ui/core/Dialog";
import DialogTitle from "[@material](http://twitter.com/material)-ui/core/DialogTitle";
import DialogContent from "[@material](http://twitter.com/material)-ui/core/DialogContent";
import DialogContentText from "[@material](http://twitter.com/material)-ui/core/DialogContentText";
import DialogActions from "[@material](http://twitter.com/material)-ui/core/DialogActions";
import Slide from "[@material](http://twitter.com/material)-ui/core/Slide";const Transition = React.forwardRef((props, ref) => {
  return <Slide direction="down" ref={ref} {...props} />;
});export default function App() {
  const [open, setOpen] = React.useState(false); const handleClose = () => {
    setOpen(false);
  }; return (
    <div>
      <Button variant="outlined" color="primary" onClick={() => setOpen(true)}>
        Open simple dialog
      </Button>
      <Dialog
        open={open}
        onClose={handleClose}
        TransitionComponent={Transition}
      >
        <DialogTitle>Confirm</DialogTitle>
        <DialogContent>
          <DialogContentText>Are you sure?</DialogContentText>
        </DialogContent>
        <DialogActions>
          <Button onClick={handleClose} color="primary">
            no
          </Button>
          <Button onClick={handleClose} color="primary" autoFocus>
            yes
          </Button>
        </DialogActions>
      </Dialog>
    </div>
  );
}
```

我们创建了`Transition`组件，它使用`Slide`组件来创建向下滑动的效果。

`direction`设定滑动的方向。

我们必须将对话框的引用传递给`Slide` prop 来使其滑动。

然后我们添加`TransitionComponent`道具，将`Transition`组件传递给它。

# 表单对话框

我们可以在对话框中添加表单。

例如，我们可以写:

```
import React from "react";
import Button from "[@material](http://twitter.com/material)-ui/core/Button";
import Dialog from "[@material](http://twitter.com/material)-ui/core/Dialog";
import DialogTitle from "[@material](http://twitter.com/material)-ui/core/DialogTitle";
import DialogContent from "[@material](http://twitter.com/material)-ui/core/DialogContent";
import DialogContentText from "[@material](http://twitter.com/material)-ui/core/DialogContentText";
import DialogActions from "[@material](http://twitter.com/material)-ui/core/DialogActions";
import TextField from "[@material](http://twitter.com/material)-ui/core/TextField";export default function App() {
  const [open, setOpen] = React.useState(false); const handleClose = () => {
    setOpen(false);
  }; return (
    <div>
      <Button variant="outlined" color="primary" onClick={() => setOpen(true)}>
        Open simple dialog
      </Button>
      <Dialog open={open} onClose={handleClose}>
        <DialogTitle>Confirm</DialogTitle>
        <DialogContent>
          <DialogContentText>Enter your name</DialogContentText>
          <TextField
            autoFocus
            margin="dense"
            id="name"
            label="name"
            type="text"
            fullWidth
          />
        </DialogContent>
        <DialogActions>
          <Button onClick={handleClose} color="primary">
            cancel
          </Button>
          <Button onClick={handleClose} color="primary" autoFocus>
            ok
          </Button>
        </DialogActions>
      </Dialog>
    </div>
  );
}
```

在我们的对话框中添加一个`TextField`。

我们用`fullWidth`支柱使其全宽，以使其适合对话框。

![](img/128410650ca03831539e6d7cee6ab18f.png)

Photo by [Alexander Dummer](https://unsplash.com/@4dgraphic?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以添加一个对话框来显示各种东西。

它可以包括按钮和表单。

也可以包括过渡效果。

## **用简单英语写的 JavaScript**

你知道我们有三份出版物和一个 YouTube 频道吗？在 [**寻找一切的链接 plainenglish.io**](https://plainenglish.io/) ！