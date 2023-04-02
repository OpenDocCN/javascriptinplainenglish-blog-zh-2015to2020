# React Hooks 应用程序中带有普通 JS 的自定义热键

> 原文：<https://javascript.plainenglish.io/custom-hot-keys-with-vanilla-js-in-a-react-hooks-app-8d17611b997a?source=collection_archive---------10----------------------->

## 跳过图书馆，滚动你自己的热键

![](img/88d1702ec6e3cf93521204dfa3b9761c.png)

Photo by [**Pixabay**](https://www.pexels.com/@pixabay?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) from [**Pexels**](https://www.pexels.com/photo/beverage-break-breakfast-brown-414630/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

在开发 React 应用程序时，我需要在用户按下 escape 键时隐藏一个组件。

谷歌快速搜索`“react custom hot keys”`推荐了一个名为`react-hotkeys`的图书馆。

不幸的是，它已经有将近一年没有被维护了(截止到 2020 年 11 月 17 日)。

所以我直接从 Droogan 的“不可维护的代码”中得到建议，并推出了自己的代码。

我是这样做的。

## 添加事件侦听器

在显示组件时，我在`keyup`事件上添加了一个事件监听器。这在按下任何键时触发。

```
window.addEventListener(
    'keyup',
    hideDisplay
)
```

`*hideDisplay*` *是我们马上要写的函数。*

## 编写一个函数供侦听器调用

然后我写了一个叫`hideDisplay`的函数，它调用`setIsShowing(false)`，并隐藏了组件。

该逻辑以被按下的`esc`键`27`为条件。

```
function hideDisplay(e) {
  var key = e.which || e.keyCode;
  if (key == 27) {setIsShowing(false*)
*  }
}
```

## 删除事件监听器

最后，我修改了上面的函数，在成功运行事件监听器的逻辑之后，删除了它。

虽然不一定与现代浏览器相关，但这是避免内存泄漏的最佳实践。

```
function hideDisplay(e) {
  var key = e.which || e.keyCode;
  if (key == 27) {setIsShowing(false)window.addEventListener(
        'keyup',
        hideDisplay
    );
  }
}
```

仅此而已。

现在，当呈现组件时，添加了一个事件监听器，它监听`esc`键的按下。当按下`esc`键时，组件被隐藏，监听器被移除。

问题解决了！

# 结论

如果你是热键新手，我希望你会发现这很有用。

我的背景不在前端，所以如果有更好的方法来实现这一点，我很乐意在评论中听到它。

也就是说，如果一个解决方案可以用不到 10 行代码实现，那么增加整个库的代码是没有意义的。

一如既往，编码快乐！