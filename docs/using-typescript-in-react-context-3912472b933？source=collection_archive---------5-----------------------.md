# 在 React 上下文中使用 TypeScript

> 原文：<https://javascript.plainenglish.io/using-typescript-in-react-context-3912472b933?source=collection_archive---------5----------------------->

TypeScript 席卷了 JavaScript 社区。曾经是无类型的编程语言，现在，通过仔细的思考，有了类型。

![](img/2ce7bf33a9f1e7b581dd44ba499975e5.png)

本文引用了这个 repo，它有一个使用 TypeScript 在应用程序中创建 React 上下文的完整示例:

[](https://gitlab.com/sundry/react/material-ui-dark-mode) [## 杂项/反应/材质-界面-深色-模式

### 材质 UI 黑暗模式

gitlab.com](https://gitlab.com/sundry/react/material-ui-dark-mode) 

TypeScript 可以接受普通的 JavaScript 并定义类型，这使得它在工具、调试和可读性方面非常出色。

我们将使用 React 上下文并添加 TypeScript，这样我们就知道在整个应用程序中可以使用什么。

```
interface ContextProps {
  darkMode: boolean;
  setDarkMode(darkMode: boolean): void;
}const Context = React.createContext<ContextProps>({
  darkMode: false,
  setDarkMode: () => {},
});
```

首先，让我们创建一个 React 上下文，它将接受一个名为 ContextProps 的 TypeScript 接口。这个接口将包括一个布尔 darkMode 和一个名为 setDarkMode 的 setter 函数。

然后，通过将 ContextProps 添加到上下文中，它将使我们能够知道我们的上下文中包含哪些变量。

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

现在，我们将创建带有 TypeScript 接口的上下文提供程序，该接口只有 React children 属性。

最后，让我们导出组件:

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

useStore 导出只是一个助手函数，我们可以在任何子组件中使用它。

withProvider 是一个助手函数，它可以将上下文提供者巧妙地包装在任何组件周围。

通过向 React 添加 TypeScript，我们可以轻松地阅读和理解应用程序可以使用的上下文。

如果你需要温习你的打字稿，你可以看看这个链接:

 [## 已经熟悉 TypeScript 了？

### 编辑描述

www.typescriptlang.org](https://www.typescriptlang.org/docs/home.html) 

打字愉快！！！