# React 自定义挂钩:useSubmit

> 原文：<https://javascript.plainenglish.io/react-custom-hook-useonesubmit-b10be17245d8?source=collection_archive---------0----------------------->

![](img/0d6132d8fef83bdcab87349aa35022ac.png)

Photo by [John Schnobrich](https://unsplash.com/@johnschno?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

对于我们大多数前端开发人员来说，我们进行 CRUD 操作，最终用户可能会对您的操作进行许多不可预测的操作。今天，我写了一个钩子，只是为了让提交按钮在异步操作完成之前无效。

```
const useSubmit = submitFunction => {
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);const handleSubmit = async () => {
    try {
      setLoading(true);
      setError(null);
      await submitFunction();
    } catch (error) {
      setError(error);
    } finally {
      setLoading(false);
    }
  };
  return [handleSubmit, loading, error];
};function App() {
  const mySubmitFunction = () => {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        const rnd = Math.random() * 10;
        rnd <= 5 ? resolve() : reject("Error occurred!");
      }, 2000);
    });
  };
  const [handleSubmit, loading, error] = useSubmit(mySubmitFunction);return (
    <div className="App">
      <button onClick={handleSubmit} disabled={loading}>
        {!loading ? "Click me" : "Loading..."}
      </button>
      {error && <div>{error}</div>}
    </div>
  );
}
```

这个钩子做什么，处理你的装载和错误状态。如果你使用 GraphQL，ApolloClient 会为你提供这些有用的信息，但是如果你是通过 RestAPI 调用你的端点，那么这个钩子可以帮助你。

[codesandbox](https://codesandbox.io/embed/dry-surf-08wz9?fontsize=14) 上的真实示例

*如果你喜欢我的文章，你可以鼓掌支持我，关注我。
我也在* [*linkedin*](http://www.linkedin.com/in/muratcatal) *上，欢迎所有邀请。*