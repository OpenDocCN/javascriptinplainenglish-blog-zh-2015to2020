# 使用 Intercept 和 Observe 观察 MobX 可观察性

> 原文：<https://javascript.plainenglish.io/watching-mobx-observables-with-intercept-and-observe-f576c9937de2?source=collection_archive---------7----------------------->

![](img/53cf573d8bc4b89a87c413d6cc6c9d36.png)

Photo by [Allef Vinicius](https://unsplash.com/@seteales?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我们可以用`intercept`函数观察 MobX 检测和修改突变，用`observe`函数观察可观察到的值变化。

在本文中，我们将了解如何使用`intercept`来监控变化，在将突变应用到可观察对象之前检测和修改它们，并使用`observe`来观察变化。

# 拦截

我们可以如下调用`intercept`函数:

```
intercept(target, propertyName?, interceptor)
```

在上面的签名中，`target`是要保护的可观察对象，`propertyName`是一个可选参数，用于指定要拦截的特定属性。

`intercept(user.name, interceptor)`与`intercept(user, "name", interceptor)`不同。第一个尝试在`user.name`中给当前的`value`添加一个拦截器，后一个拦截器改变了`user`的`name`属性。

`interceptor`是一个回调函数，它将在每次对可观察对象进行更改时运行。

`intercept`函数告诉 MobX 当前的更改需要做什么。

它应该执行下列操作之一:

*   从函数中原样返回接收到的`change`对象
*   修改`change`对象并返回
*   返回`null`表示不应应用更改
*   抛出异常

`intercept`返回一个`disposer`函数，当拦截器被调用时，这个函数可以用来取消拦截器。

可以将多个拦截器注册到同一个可观察对象。他们将在注册顺序中被链接。

如果其中一个拦截器返回`null`或抛出异常，那么其他拦截器将不再被评估。

也可以在父对象和单个属性上注册一个拦截器。

例如，我们可以如下使用它:

```
import { observable, intercept } from "mobx";const theme = observable({
  color: "#ffffff"
});const disposer = intercept(theme, "color", change => {
  if (!change.newValue) {
    return null;
  }
  if (change.newValue.length === 6) {
    change.newValue = `#${change.newValue}`;
    return change;
  }
  if (change.newValue.length === 7) {
    return change;
  }
  if (change.newValue.length > 10) {
    disposer();
  }
  throw new Error(`Invalid color`);
});
```

在上面的代码中，我们为`theme`可观察对象的`color`属性定义了一个拦截器。

拦截器回调接受一个带有传入值的`change`参数。然后，它运行一些检查，并对`newValue`属性进行一些更改。

# 观察

`observe`功能用于观察可观察值的变化。

可以这样称呼它:

```
observe(target, propertyName?, listener, invokeImmediately?)
```

在上面的签名中，`target`是可以观察到的。

`propertyName`是一个可选参数，用于指定要观察的属性。`observe(user.name, listener)`不同于`observe(user, "name", listener)`。前者在`user.name`观察当前的`value`，后者观察用户的`name`属性。

`listener`是一个回调函数，每次对可观察对象进行修改时都会被调用。它接收一个描述突变的 change 对象，除了 boxed observables，它将调用一个带有`newValue`和`oldValue`参数的监听器。

`invokeImmediately`是默认为`false`的布尔值。如果我们希望`observe`用可观察的状态直接调用`listener`，而不是等待第一次变化，则将它设置为`true`。

例如，我们可以如下使用它:

```
import { observable, observe } from "mobx";const person = observable({
  firstName: "John",
  lastName: "Smith"
});const disposer = observe(person, change => {
  console.log(
    `${change.type} ${change.name} from ${change.oldValue} to ${
      change.object[change.name]
    }`
  );
});person.firstName = "Jane";
```

在上面的代码中，我们有一个回调函数，回调函数中的`change`对象包含`type`变更、`name`变更、`oldValue`和`change.object[change.name]`以获取新值。

# 事件

`intercept`和`observe`的回调将接收一个事件对象，该对象包含触发事件的可观察对象的`object`和当前事件类型的`type`字符串。

根据类型，它们还有其他字段:

## 对象添加事件

`add`事件为我们提供了`name`和`newValue`属性，分别表示添加的属性名称和分配的新值。

![](img/fa4fee8e7d6d07ed432ef637b798be3b.png)

Photo by [britt gaiser](https://unsplash.com/@brittanyg?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 对象更新事件

`update`事件为我们提供了`name`和`newValue`属性，分别表示添加的属性名称和分配的新值。

此外，它为我们提供了被替换的值的`oldValue`属性。

## 阵列拼接事件

这为拼接的起始索引提供了`index`。拼接也可通过其他排列方式进行，如`push`等。

`removedCount`给出被移除的物品数量。

`added`给我们一个正在添加的项目数组。

`removed`给出了一个添加了条目的数组。

`addedCount`给出添加的项目数量。

## 数组更新事件

更新事件给了我们`index`,它拥有被更新的单个条目的索引

给我们被赋值的值。

`oldVale`具有被替换的旧值。

## 映射添加和删除事件

Map add 和 delete 事件具有分别用于添加的属性名称和分配的新值的`name`和`newValue`属性。

## 地图更新事件

delete 事件为我们提供了与 add 和 delete 事件一起发送的属性，还提供了带有被替换值的`oldValue`。

## 装箱和计算的可观测量创建事件

装箱的和计算的 observables 的 create 事件具有`newValue`属性来获取我们在创建过程中分配的值。

## 装箱和计算的可观测量更新事件

除了`newValue`之外，更新事件还给出了值被替换的`oldValue`。

# 结论

我们可以使用`intercept`函数在可观察状态的突变完成之前做一些事情。

`observe`函数可用于观察 MobX 可观察值的变化。它需要一个回调，给我们状态的新旧值和其他信息。

## **用简单英语写的 JavaScript 的注释:**

我们已经推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**