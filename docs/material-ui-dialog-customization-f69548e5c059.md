# 材料用户界面—对话框定制

> 原文：<https://javascript.plainenglish.io/material-ui-dialog-customization-f69548e5c059?source=collection_archive---------1----------------------->

![](img/f770287fd24630a2b95b6db63ed7753b.png)

Photo by [Shan Li Fang](https://unsplash.com/@fangshanli?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

材质 UI 是一个为 React 制作的材质设计库。

这是一组具有材质设计风格的 React 组件。

在这篇文章中，我们将看看如何用材质 UI 定制对话框。

# 定制对话框

我们可以创建自己的对话框组件，方法是放入我们自己的组件，并向它传递各种样式。

例如，我们可以写:

```
import React from "react";
import { withStyles } from "[@material](http://twitter.com/material)-ui/core/styles";
import Button from "[@material](http://twitter.com/material)-ui/core/Button";
import Dialog from "[@material](http://twitter.com/material)-ui/core/Dialog";
import MuiDialogTitle from "[@material](http://twitter.com/material)-ui/core/DialogTitle";
import MuiDialogContent from "[@material](http://twitter.com/material)-ui/core/DialogContent";
import MuiDialogActions from "[@material](http://twitter.com/material)-ui/core/DialogActions";
import IconButton from "[@material](http://twitter.com/material)-ui/core/IconButton";
import CloseIcon from "[@material](http://twitter.com/material)-ui/icons/Close";
import Typography from "[@material](http://twitter.com/material)-ui/core/Typography";const styles = theme => ({
  root: {
    margin: 0,
    padding: theme.spacing(3)
  },
  closeButton: {
    position: "absolute",
    right: theme.spacing(1),
    top: theme.spacing(2),
    color: theme.palette.grey[500]
  }
});const DialogTitle = withStyles(styles)(props => {
  const { children, classes, onClose, ...other } = props;
  return (
    <MuiDialogTitle disableTypography className={classes.root} {...other}>
      <Typography variant="h6">{children}</Typography>
      {onClose ? (
        <IconButton className={classes.closeButton} onClick={onClose}>
          <CloseIcon />
        </IconButton>
      ) : null}
    </MuiDialogTitle>
  );
});const DialogContent = withStyles(theme => ({
  root: {
    padding: theme.spacing(2)
  }
}))(MuiDialogContent);const DialogActions = withStyles(theme => ({
  root: {
    margin: 0,
    padding: theme.spacing(1)
  }
}))(MuiDialogActions);export default function App() {
  const [open, setOpen] = React.useState(false); const handleClickOpen = () => {
    setOpen(true);
  };
  const handleClose = () => {
    setOpen(false);
  }; return (
    <div>
      <Button variant="outlined" color="primary" onClick={handleClickOpen}>
        Open dialog
      </Button>
      <Dialog onClose={handleClose} open={open}>
        <DialogTitle onClose={handleClose}>Modal</DialogTitle>
        <DialogContent dividers>
          <Typography gutterBottom>lorem ipsum.</Typography>
        </DialogContent>
        <DialogActions>
          <Button autoFocus onClick={handleClose} color="primary">
            ok
          </Button>
        </DialogActions>
      </Dialog>
    </div>
  );
}
```

我们创建了一个`styles`函数，并将其传递给具有不同风格的`withStyles`高阶组件。

我们通过为`closeButton`类设置一些样式来移动关闭按钮。

为了创建`DialogTitle`组件，我们传递了一个使用 props 中的类和子类的组件。

这些类应用于图标按钮和对话框标题。

此外，我们以类似的方式创建了`DialogContent`和`DialogActions`。

我们稍微改变了填充以创建两个组件。

然后我们把它们都用在了`App`组件中。

# 全屏对话框

我们可以通过给`Dialog`添加`fullscreen`道具来全屏显示对话框。

例如，我们可以写:

```
import React from "react";
import Button from "[@material](http://twitter.com/material)-ui/core/Button";
import Dialog from "[@material](http://twitter.com/material)-ui/core/Dialog";
import DialogTitle from "[@material](http://twitter.com/material)-ui/core/DialogTitle";
import DialogContent from "[@material](http://twitter.com/material)-ui/core/DialogContent";
import DialogContentText from "[@material](http://twitter.com/material)-ui/core/DialogContentText";
import DialogActions from "[@material](http://twitter.com/material)-ui/core/DialogActions";export default function App() {
  const [open, setOpen] = React.useState(false);
  const handleClose = () => {
    setOpen(false);
  };
  return (
    <div>
      <Button variant="outlined" color="primary" onClick={() => setOpen(true)}>
        Open dialog
      </Button>
      <Dialog fullScreen open={open} onClose={handleClose}>
        <DialogTitle>Title</DialogTitle>
        <DialogContent>
          <DialogContentText>lorem ipsum</DialogContentText>
        </DialogContent>
        <DialogActions>
          <Button onClick={handleClose} color="primary" autoFocus>
            ok
          </Button>
        </DialogActions>
      </Dialog>
    </div>
  );
}
```

用`fullScreen`道具创建一个全屏的对话框。

# 可选尺寸

我们可以用`maxWidth`道具改变对话框的大小来设置对话框的最大宽度。

`fullWidth`具有全幅。

例如，我们可以写:

```
import React from "react";
import Button from "[@material](http://twitter.com/material)-ui/core/Button";
import Dialog from "[@material](http://twitter.com/material)-ui/core/Dialog";
import DialogTitle from "[@material](http://twitter.com/material)-ui/core/DialogTitle";
import DialogContent from "[@material](http://twitter.com/material)-ui/core/DialogContent";
import DialogContentText from "[@material](http://twitter.com/material)-ui/core/DialogContentText";
import DialogActions from "[@material](http://twitter.com/material)-ui/core/DialogActions";export default function App() {
  const [open, setOpen] = React.useState(false);
  const handleClose = () => {
    setOpen(false);
  };
  return (
    <div>
      <Button variant="outlined" color="primary" onClick={() => setOpen(true)}>
        Open dialog
      </Button>
      <Dialog fullWidth maxWidth="sm" open={open} onClose={handleClose}>
        <DialogTitle>Title</DialogTitle>
        <DialogContent>
          <DialogContentText>lorem ipsum</DialogContentText>
        </DialogContent>
        <DialogActions>
          <Button onClick={handleClose} color="primary" autoFocus>
            ok
          </Button>
        </DialogActions>
      </Dialog>
    </div>
  );
}
```

给`Dialog`增加`fullWidth`和`maxWidth`支柱。

如果对话框比`maxWidth`中给出的断点窄，那么`fullWidth`道具将显示延伸到屏幕宽度的对话框。

否则，它将显示给定断点的最大宽度。

![](img/0cc2c7b5aa666dd1fb00a2abe06510a7.png)

Photo by [Xavier crook](https://unsplash.com/@profxavier26?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以添加具有自己风格的对话框，全屏对话框，也可以设置最大宽度。

## **简单英语的 JavaScript**

你知道我们有三份出版物和一个 YouTube 频道吗？在[**plain English . io**](https://plainenglish.io/)找到一切的链接！