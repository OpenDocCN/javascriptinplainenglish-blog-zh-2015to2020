# 使用自定义 JavaScript 错误

> 原文：<https://javascript.plainenglish.io/using-custom-javascript-errors-39270273fc2b?source=collection_archive---------6----------------------->

## 以及为什么它们在编程时很有用

![](img/093e45934884dffae4590bd3dd1a417f.png)

Photo by [Billy Pasco](https://unsplash.com/@billy_pasco?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

您应该对这段代码很熟悉:

```
function validateUser({ userId, password }) {
  try {
    axios.post('https://my-login-endpoint', { userId, password });
  } catch (e) {
    if (e.response.status === 400) {
      displayError(e.response.data.detail);
    } else {
      displayError("Something went wrong."); // Generic error
    }
  }
}
```

displayError 是一个自定义函数，我假设您已经编写了这个函数，它要么直接在模态中显示错误，要么如果您正在使用 React，它会调度另一个动作，将您的一个 reducers 中的 Error 属性设置为`e.response.data.detail`或相应的一般错误。如果您正在使用`redux-saga`，这种 try catch 模式尤其常见。唯一的区别是这个函数是一个生成器函数。

但是假设`userId`只能是一个 UUID/GUID。如果输入的用户 Id 不是有效的 UUID，那么向端点发送请求值得吗？显然不是因为我们意识到它将被拒绝，当我们知道响应是什么时，增加服务器负载是徒劳的。那么我们检查一下`userId`是否是有效的 UUID，如果不是，我们用我们选择的字符串调用`displayError`。这很容易做到，但这不是最佳的软件设计，因为我们已经有了显示错误的逻辑，为什么不使用它呢？但是如果我们`throw new Error()`如果`userId`不是一个有效的 UUID 会发生什么呢？我们将得到著名的 javascript 错误:`Cannot read property status of undefined`，因为[错误对象](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Error)没有响应属性。所以这是一个自定义错误的好用例。这是一种简单的方法:

```
function validateUser({ userId, password }) {
  try {
    if (!isValidUUID(userId)) {
      const badRequestError = new ***Error***('BadRequest');
      badRequestError.response = {
        status: 400,
        data: {
          detail: 'Please enter a valid UUID!'
        }
      };
      throw badRequestError;
    }
    axios.post('my-endpoint', { userId, password });
  } catch (e) {
    if (e.response.status === 400) {
      displayError(e.response.data.detail);
    } else {
      displayError("Something went wrong."); // Generic error
    }
  }
}
```

上面的代码满足条件`e.response.status === 400`，因此我们使用`displayError`函数的错误细节。

显然，最好的方法是在用户表单中进行验证，并保持“登录”按钮禁用，直到用户输入有效的 UUID。但是，如果您必须调用 API 来验证请求是否会失败呢？例如，如果您必须先执行 google recaptcha 函数，然后调用您的登录 API。在表单输入的 onchange 处理程序上调用 google-recaptcha 端点是不可行的，最好留给表单的 submit 函数。在这里，您可以调用 google-recaptcha 函数，并相应地选择是要命中登录 API 端点还是只显示一个错误。

我希望你觉得这很有用。请关注我，了解更多类似的内容以及软件的商业应用。下次见！