# 反应材质 UI 黑暗模式

> 原文：<https://javascript.plainenglish.io/react-material-ui-dark-mode-705f33202344?source=collection_archive---------4----------------------->

## 创建 React 应用程序，该应用程序使用材质 UI 在黑暗模式下进行渲染

![](img/2ce7bf33a9f1e7b581dd44ba499975e5.png)

随着互联网的出现，人们总是盲目地打开应用程序，无论是在手机上还是在电脑上。进入黑暗模式。

黑暗模式已经变得越来越流行。如果你的应用或网站有黑暗模式，那么你已经赢得了这场战斗。用 React 和 Material UI 很容易实现。它会用深色和对比强烈的文字来渲染组件，这样看起来很舒服。

在本文中，我们将从头开始，创建一个 react 应用程序，安装 Material UI，并设置一个上下文来存储我们的主题状态。

本文的来源可以在这里找到。

[](https://gitlab.com/sundry/react/material-ui-dark-mode) [## 杂项/反应/材质-界面-深色-模式

### 材质 UI 黑暗模式

gitlab.com](https://gitlab.com/sundry/react/material-ui-dark-mode) 

首先，我们将使用 React 和 TypeScript。你可以使用 npm 或者 yarn。

```
npx create-react-app my-app — template typescript
```

下面是 React 网站上安装 React 的命令。

[](https://create-react-app.dev/docs/adding-typescript/) [## 创建 React 应用程序通过运行一个命令设置一个现代化的 web 应用程序。

### 注意:此功能在“react-scripts@2.1.0”和更高版本中可用。

创建-反应-应用程序.开发](https://create-react-app.dev/docs/adding-typescript/) 

好了，现在我们已经安装了 React。让我们创建一个 React 上下文来存储用户的主题状态。不是所有人都喜欢黑暗模式，所以我们将默认它是假的。

```
interface ContextProps {
  darkMode: boolean;
  setDarkMode(darkMode: boolean): void;
}const Context = React.createContext<ContextProps>({
  darkMode: false,
  setDarkMode: () => {},
});
```

这将创建一个 React 上下文，我们可以在其中存储一个名为 darkMode 的布尔变量和一个名为 setDarkMode 的 setter 函数，它可以在亮暗模式之间切换。

现在我们将创建一个 React 上下文提供者，我们可以在应用程序中的任何地方使用它。

```
interface Props {
  children?: React.ReactNode;
}const Provider: React.FC<Props> = ({ children }) => {
  const [darkMode, setDarkMode] = useLocalStorage('darkMode', false);
  return (
    <Context.Provider
      value={{
        darkMode,
        setDarkMode,
      }}
    >
    {children}
  </Context.Provider>
 );
};
```

该代码片段将创建一个 React 上下文提供程序，该提供程序将使用一个名为 useLocalStorage 的自定义挂钩。

查看我关于如何创建自己的定制钩子的文章，尤其是 useLocalStorage 钩子。

[](https://medium.com/@andrewgbliss/react-custom-hook-uselocalstorage-afbde976c72b) [## React 自定义挂钩 useLocalStorage

### React Hooks 风靡全球！不需要笨重的类和奇怪的关键字 this。现在我们…

medium.com](https://medium.com/@andrewgbliss/react-custom-hook-uselocalstorage-afbde976c72b) 

现在让我们开始导出我们的精彩上下文，这样我们就可以在我们的应用程序中使用它们了。

```
export const useStore = () => useContext(Context);export function withProvider(Component: any) {
  return function WrapperComponent(props: any) {
    return (
      <Provider>
        <Component {...props} />
      </Provider>
    );
  };
}export { Context, Provider };
```

这将导出上下文和提供者，以便在我们的应用程序中使用。但是，让我们也导出一些助手函数，这样我们就可以整齐地包装 or 组件。

useStore 函数是我们进入这个上下文的钩子。我们将在我们的组件中使用它来切换主题。

withProvider 函数是我们的包装器，它将使我们能够在这个上下文中整齐地包装我们的应用程序组件。如果您不熟悉 React 上下文，这里有一个链接。

[https://reactjs.org/docs/hooks-reference.html#usecontext](https://reactjs.org/docs/hooks-reference.html#usecontext)

让我们再添加几个助手函数。

```
export const useApp = () => {
  const { darkMode, setDarkMode } = useStore();
  return {
    darkMode,
    setDarkMode,
  };
};export function withThemeProvider(Component: any) {
  const WrapperComponent = ({ props }: any) => {
  const { darkMode } = useApp();
  const theme = darkMode ? darkTheme : defaultTheme;
  return (
    <ThemeProvider theme={theme}>
      <CssBaseline />
      <Component {...props} />
    </ThemeProvider>
  );
 };
 return withProvider(WrapperComponent);
}
```

useApp 函数只是将 useStore 函数重命名为更容易理解的名称。这是我们的应用环境。每当主题改变，它需要更新整个应用程序。当然，您不希望经常改变这种状态。你可能想使用 Memo 和 useCallback 来提高应用程序的效率，但这是一个更广泛的话题，我们将在后面的文章中讨论。

withThemeProvider 函数是一个包装器，它使我们能够将我们的应用程序包装在一个材质 UI 主题中。你可以看到我正在使用 useApp 钩子并提取 darkMode 布尔值来决定材质 UI 使用哪个主题。当变量状态改变时，它会用新的主题呈现这个组件。

现在让我们看看我们简单的应用程序组件。

```
import React from 'react';
import { makeStyles } from '@material-ui/core/styles';
import { withThemeProvider } from './store/Store';
import Grid from '@material-ui/core/Grid';
import Paper from '@material-ui/core/Paper';
import Box from '@material-ui/core/Box';
import FormControlLabel from '@material-ui/core/FormControlLabel';
import Switch from '@material-ui/core/Switch';
import { useStore } from './store/Store';const useStyles = makeStyles(theme => ({
  root: {
    width: '100%',
    height: '100%',
  },
}));const App: React.FC = () => {
  const classes = useStyles();
  const { darkMode, setDarkMode } = useStore();
  return (
    <Grid
      className={classes.root}
      container
      justify="center"
      alignItems="center"
    >
      <Paper>
        <Box p={10}>
          <FormControlLabel
            control={
              <Switch
                checked={darkMode}
                onChange={() => setDarkMode(!darkMode)}
              />
            }
            label="Dark Mode"
          />
       </Box>
     </Paper>
   </Grid>
 );
};export default withThemeProvider(App);
```

这是我们的应用程序组件，它将接受我们的 useStore 挂钩，并从上下文中获取 darkMode 和 setDarkMode。

它将呈现一个用户可以用来打开或关闭黑暗模式主题的开关组件。

您可能希望将此开关放在抽屉或用户设置页面中。一旦你有了上下文，我们将应用程序包装在我们的 withThemeProvider 中。

再一次，你应该记住，当创建一个像这样的全球环境时，你的应用程序的性能。全局上下文应该只存储关于整个应用程序的信息，比如主题或登录的用户。

谢谢！