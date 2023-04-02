# 将上下文 API 与高阶组件进行反应

> 原文：<https://javascript.plainenglish.io/react-context-api-with-higher-order-component-279d5f93eda4?source=collection_archive---------2----------------------->

正如大多数人已经知道的那样，我们可以使用上下文 API 将道具从父组件传递到其子组件或孙组件，而无需通过每个子组件向下钻取(传递)道具。

## 故事时间:

就好像我的曾祖父保存了一个宝藏，并告诉了整个家族。所以他的任何血统都可以访问它，而不需要得到他们上一代(也就是父母)的许可。

![](img/aa21d8959c1d638fafef6fc5612cbc3b.png)

I don’t need to ask my Dad ?

他非常有创造力，他为家庭定制了更具体的宝藏，增加了功能(即定制提供商)，因此如果他们需要使用魔法钥匙(即定制消费者)，他的血统可以利用它。

## 现实:

让我们使用上下文 API 来创建我们讨论过自定义宝藏和魔术密钥。

## 创建上下文:

我们将确保 react contextAPI 创建神奇的宝盒。我们可以在声明`createContext`时传递默认值

## 声明提供者:

这将作为我们的应用程序可以使用的自定义提供程序。他们所要做的就是用`<TreasureProvider>`包装他们的根组件，并传递 props。在我们的例子中，他们必须通过`asset`。

## 添加额外功能

这是可选的 javascript 类，它有一些助手方法，我们可以将这些方法传递给我们的`<TreasureProvider>`的消费者。在子组件中(即消费者)可以访问这个类的任何方法。

## 创建特设消费者(魔术钥匙)

接受 WrappedComponent 并从 TreasureBox 注入 props 的高阶组件。因此，每当子组件用`withMagicKey`包装时，它会自动从`<TreasureProvider>`获取所有细节。

## 在应用程序中使用它:

现在让我们在实际应用中使用我们刚刚创建的特定上下文 API。为了简单起见，我将只发布根组件，即`App.js`和子组件`FirstChild.js`。

用`<TreasureProvider>`包装`App.js`，这样我们就可以让 contextAPI 对整个应用程序可用。请记住，我们在用`<TreasureProvider>`包装时会将`asset`作为道具传递。

现在我们已经添加了自定义提供者，我们可以通过用`withMagicKey`包装组件来从子组件或孙组件中检索上下文

## 控制台输出:

```
Anderson's family has asset worth of $200000000  CustomFeature.js:18
```