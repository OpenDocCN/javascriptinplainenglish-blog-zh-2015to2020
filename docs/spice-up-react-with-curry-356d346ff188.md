# 香料与成分和 Currying 反应

> 原文：<https://javascript.plainenglish.io/spice-up-react-with-curry-356d346ff188?source=collection_archive---------7----------------------->

## 避免函数匹配和组合的属性冲突

![](img/0cf406d64ffec3486d0fbb7bc848e218.png)

Photo by [emy](https://unsplash.com/@grimnoire?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/spicy-curry?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

反应高阶组件(HOC)没有失效；事实上，如果使用得当，它们是一个方便的工具。Eric Elliot 有关于何时使用 HOCs vs. Hooks 的[权威指南](https://medium.com/javascript-scene/do-react-hooks-replace-higher-order-components-hocs-7ae4a08b7b58)。然而，一个经常被忽视的情况是如何在您的 HOC 中使用钩子的结果。仅仅为了在组件属性中传递钩子调用的结果而添加一个 HOC 是一种反模式。通过属性传递可能会迅速膨胀，导致命名冲突以及其他负面影响。为了避免这种情况，可以使用函数 currying。

> 注意:这些例子使用了[流](https://flow.org/)类型，但是同样适用于[类型脚本](https://www.typescriptlang.org/)，且在语法上几乎相同。如果您不理解[泛型](https://flow.org/en/docs/types/generics/)，请仔细阅读它们，因为它们在本文中被广泛使用。

## 反模式:用 HOCs 作曲

```
export const **withQuestionDataPreload** = compose(
  **withDataPreload**<ServiceCallParamsType>(
    ({apolloClient, routeParams}: ServiceCallParamsType) => {
      return apolloClient.query({
        query: QUESTIONS_QUERY,
        variables: {
          questionId: parseInt(routeParams.questionId, 10),
        },
      });
    }
  ),
  **withRouteParams**,
  **withApolloClient**,
);
```

在本例中， [ramda compose](https://ramdajs.com/docs/#compose) 用于创建一个包含数据预加载的 HOC。数据预加载是通过调用`withDataPreload`返回的服务并与`React.lazy`并行调用来实现的

```
export const **withDataPreload** = <Config>(
  **serviceCall: (params: Config) => mixed**
): ((component: ComponentPromiseType<Config>) => AbstractComponent<Config>) => (
  component: ComponentPromiseType<Config>
): AbstractComponent<Config> => {
  function ViewComponent(**props: Config**) {
    React.useEffect(() => {
      **serviceCall(props);**
    }, []);

    const ComponentPromise = **React.lazy(component)**;

    return <ComponentPromise {...props} />;
  }

  ViewComponent.displayName = `withDataPreload(${getDisplayName(
    ViewComponent
  )})`;

  return ViewComponent;
};
```

这里的问题是所有的`serviceCall`依赖都必须作为**组件属性**来传递。这可能导致属性的大量累积，并导致命名冲突。

## 解决方法:[函数 curry](https://medium.com/javascript-scene/curry-and-function-composition-2c208d774983)

```
export const hooks = function<T>(...hooks): () => T {
  return (): T => {
    return hooks.reduce((params, hook) => {
      return {...params, ...hook()};
    });
  };
};
```

为了解决这个问题，我们创建了一个函数，它将通过使用一种形式化的`reduce`执行钩子来创建我们的参数。我们定义了泛型`T`来允许类型系统为调用者提供类型安全。

我们现在可以在没有大量属性的情况下创建我们的 HOC，并符合 Eric Elliot 对 HOC 使用的测试:

```
export type ServiceCallParamsType = {
  apolloClient: ApolloClient,
  routeParams: {
    questionId: string,
  },
};export const withQuestionDataPreload= compose(
  withDataPreload<ServiceCallParamsType>(
    **hooks<ServiceCallParamsType>(useParams, useApolloClient, useConfig)**,
    ({params}: ServiceCallParamsType) => {
      return **params.apolloClient.query**({
        query: QUESTIONS_QUERY,
        variables: {
          questionId: parseInt(**params.questionId**, 10),
        },
      });
    }
  )
);
```

这里并不要求使用 compose，但是我通常喜欢把它放在适当的位置，原因和我们使用尾随逗号的原因一样。如果已经有了组合管道，就更容易扩展它。

然后`withDataPreload`变成

```
type ComponentPromiseType<Config> = () => Promise<{
  default: AbstractComponent<Config>,
}>;

export const withDataPreload = <Config>(
  **getParams: () => Config**,
  **serviceCall: (params: Config) => mixed**
): ((component: ComponentPromiseType<Config>) => AbstractComponent<Config>) => (
  component: ComponentPromiseType<Config>
): AbstractComponent<Config> => {
  function ViewComponent(props: Config) {
    **const params = getParams();**

    React.useEffect(() => {
      **serviceCall(params);**
    }, []);

    const ComponentPromise = **React.lazy(component)**;

    return <ComponentPromise {...props} />;
  }

  ViewComponent.displayName = `withDataPreload(${getDisplayName(
    ViewComponent
  )})`;

  return ViewComponent;
```

密切注意`getParams`呼叫的位置。因为我们使用了`hooks`来搜索结果，所以可以在正确的位置使用`getParams`,以避免 React 产生无效的钩子调用异常。

我们现在有了一个非常可重用的方法来组装 hoc 的依赖项，而不需要创建大量的属性！