# 材料界面—菜单定制

> 原文：<https://javascript.plainenglish.io/material-ui-menu-customization-13923a40ad25?source=collection_archive---------3----------------------->

![](img/249f81466845245551b2398f4be449a0.png)

fFPhoto by [Ola Mishchenko](https://unsplash.com/@olamishchenko?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

材质 UI 是一个为 React 制作的材质设计库。

这是一组具有材质设计风格的 React 组件。

在这篇文章中，我们将看看如何定制菜单的材料用户界面。

# 定制菜单

我们可以根据自己的风格定制菜单。

例如，我们可以写:

```
import React from "react";
import { withStyles } from "[@material](http://twitter.com/material)-ui/core/styles";
import Button from "[@material](http://twitter.com/material)-ui/core/Button";
import Menu from "[@material](http://twitter.com/material)-ui/core/Menu";
import MenuItem from "[@material](http://twitter.com/material)-ui/core/MenuItem";
import ListItemIcon from "[@material](http://twitter.com/material)-ui/core/ListItemIcon";
import ListItemText from "[@material](http://twitter.com/material)-ui/core/ListItemText";
import InboxIcon from "[@material](http://twitter.com/material)-ui/icons/MoveToInbox";const StyledMenu = withStyles({
  paper: {
    border: "1px solid #d3d4d5"
  }
})(props => (
  <Menu
    elevation={0}
    getContentAnchorEl={null}
    anchorOrigin={{
      vertical: "bottom"
    }}
    transformOrigin={{
      vertical: "top"
    }}
    {...props}
  />
));const StyledMenuItem = withStyles(theme => ({
  root: {
    "&:focus": {
      backgroundColor: theme.palette.primary.main,
      "& .MuiListItemIcon-root, & .MuiListItemText-primary": {
        color: theme.palette.common.white
      }
    }
  }
}))(MenuItem);export default function App() {
  const [anchorEl, setAnchorEl] = React.useState(null); const handleClick = event => {
    setAnchorEl(event.currentTarget);
  }; const handleClose = () => {
    setAnchorEl(null);
  }; return (
    <div>
      <Button variant="contained" color="primary" onClick={handleClick}>
        Open Menu
      </Button>
      <StyledMenu open={Boolean(anchorEl)}>
        <StyledMenuItem onClick={handleClose}>
          <ListItemIcon>
            <InboxIcon fontSize="small" />
          </ListItemIcon>
          <ListItemText primary="home" />
        </StyledMenuItem>
      </StyledMenu>
    </div>
  );
}
```

使用`withStyles`高阶组件来设计菜单样式。

为了样式化菜单项，我们可以用`withStyles`高阶组件来样式化它。

我们用`theme`参数中的一个来设置颜色。

然后我们可以在我们的`App`组件中使用它们。

# 最大高度菜单

我们可以用`style`属性设置`Menu`的高度。

例如，我们可以写:

```
import React from "react";
import Button from "[@material](http://twitter.com/material)-ui/core/Button";
import Menu from "[@material](http://twitter.com/material)-ui/core/Menu";
import MenuItem from "[@material](http://twitter.com/material)-ui/core/MenuItem";const options = ["apple", "orange", "grape", "banana", "pear", "mango"];export default function App() {
  const [anchorEl, setAnchorEl] = React.useState(null); const handleClick = event => {
    setAnchorEl(event.currentTarget);
  }; const handleClose = () => {
    setAnchorEl(null);
  }; return (
    <div>
      <Button variant="contained" color="primary" onClick={handleClick}>
        Open Menu
      </Button>
      <Menu
        anchorEl={anchorEl}
        keepMounted
        open={Boolean(anchorEl)}
        onClose={handleClose}
        PaperProps={{
          style: {
            maxHeight: `200px`
          }
        }}
      >
        {options.map(option => (
          <MenuItem
            key={option}
            selected={option === "apple"}
            onClick={handleClose}
          >
            {option}
          </MenuItem>
        ))}
      </Menu>
    </div>
  );
}
```

我们在`PaperProps`中设置`maxHeight`属性来设置菜单的高度。

# 改变过渡

我们可以在菜单打开时添加过渡。

需要一个`TransitionComponent`道具让我们添加想要的过渡。

例如，我们可以写:

```
import React from "react";
import Button from "[@material](http://twitter.com/material)-ui/core/Button";
import Menu from "[@material](http://twitter.com/material)-ui/core/Menu";
import MenuItem from "[@material](http://twitter.com/material)-ui/core/MenuItem";
import Fade from "[@material](http://twitter.com/material)-ui/core/Fade";export default function App() {
  const [anchorEl, setAnchorEl] = React.useState(null);
  const open = Boolean(anchorEl); const handleClick = event => {
    setAnchorEl(event.currentTarget);
  }; const handleClose = () => {
    setAnchorEl(null);
  }; return (
    <div>
      <Button onClick={handleClick}>Open</Button>
      <Menu
        anchorEl={anchorEl}
        keepMounted
        open={open}
        onClose={handleClose}
        TransitionComponent={Fade}
      >
        <MenuItem onClick={handleClose}>home</MenuItem>
        <MenuItem onClick={handleClose}>profile</MenuItem>
        <MenuItem onClick={handleClose}>logout</MenuItem>
      </Menu>
    </div>
  );
}
```

当菜单打开且`TransitionComponent`道具设置为`Fade`时，添加渐变过渡。

# 上下文菜单

要添加上下文菜单，我们可以听一下`onContextMenu`道具。

然后，我们可以通过在`anchorPosition`属性中设置鼠标坐标来显示右键单击的上下文菜单。

例如，我们可以写:

```
import React from "react";
import Menu from "[@material](http://twitter.com/material)-ui/core/Menu";
import MenuItem from "[@material](http://twitter.com/material)-ui/core/MenuItem";
import Typography from "[@material](http://twitter.com/material)-ui/core/Typography";const initialState = {
  mouseX: null,
  mouseY: null
};
export default function App() {
  const [state, setState] = React.useState(initialState); const handleClick = event => {
    event.preventDefault();
    setState({
      mouseX: event.clientX - 2,
      mouseY: event.clientY - 4
    });
  }; const handleClose = () => {
    setState(initialState);
  }; return (
    <div onContextMenu={handleClick} style={{ cursor: "context-menu" }}>
      <Typography>
        Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nullam ipsum
        purus, bibendum sit amet vulputate eget, porta semper ligula. Donec
        bibendum vulputate erat, ac fringilla mi finibus nec. Donec ac dolor sed
        dolor porttitor blandit vel vel purus. Fusce vel malesuada ligula. Nam
        quis vehicula ante, eu finibus est. Proin ullamcorper fermentum orci,
        quis finibus massa.
      </Typography>
      <Menu
        keepMounted
        open={state.mouseY !== null}
        onClose={handleClose}
        anchorReference="anchorPosition"
        anchorPosition={
          state.mouseY !== null && state.mouseX !== null
            ? { top: state.mouseY, left: state.mouseX }
            : undefined
        }
      >
        <MenuItem onClick={handleClose}>select</MenuItem>
        <MenuItem onClick={handleClose}>paste</MenuItem>
        <MenuItem onClick={handleClose}>copy</MenuItem>
        <MenuItem onClick={handleClose}>save</MenuItem>
      </Menu>
    </div>
  );
}
```

添加包含一些文本的 div。

我们添加一个带有`Menu`组件的上下文菜单。

我们添加了`keepMounted`来将它保存在 DOM 中。

`mouseY`不是`null`时`open`就是`true`。

当我们在`handleClick`功能中右击时，它被设置。

`anchorPosition`让我们设置`mousex`和`mouseY`来用给定的鼠标坐标打开菜单。

![](img/48bcfb384d7750412b967462bb711c25.png)

Photo by [Michelle Tresemer](https://unsplash.com/@mtresemer?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以添加带有样式的菜单。

此外，我们可以添加上下文菜单。

当菜单打开时，还可以添加过渡来改变效果。

## **简单英语的 JavaScript**

你知道我们有三份出版物和一个 YouTube 频道吗？在[**plain English . io**](https://plainenglish.io/)找到一切的链接！