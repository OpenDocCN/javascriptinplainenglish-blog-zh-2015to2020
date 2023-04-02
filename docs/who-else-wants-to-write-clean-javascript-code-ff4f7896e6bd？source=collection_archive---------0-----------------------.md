# 还有谁想写干净的 JavaScript 代码？

> 原文：<https://javascript.plainenglish.io/who-else-wants-to-write-clean-javascript-code-ff4f7896e6bd?source=collection_archive---------0----------------------->

## 让你的同事爱上你的代码的 7 个技巧。

![](img/b9143a81e1bf93066341cf4738518733.png)

Photo by [Christopher Robin Ebbinghaus](https://unsplash.com/@cebbbinghaus?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

你的学院:“这个代码的作者是谁？”

期待:“是我！”你骄傲地回答，因为那段代码美得像公主。

现实:“不，不是我！”你撒谎是因为代码丑得像头野兽。

现在，如果你想让期望变成现实，继续读下去。

# 1.使用有意义的变量名

使用有意义的名字，一看就知道是什么。

```
**// Don't**
let xyz = validate(‘amyjandrews’);**// Do**
let isUsernameValid = validate(‘amyjandrews’);
```

将集合类型命名为复数是有意义的。因此，不要忘记 **s** :

```
**// Don't**
let number = [3, 5, 2, 1, 6];**// Do**
let numbers = [3, 5, 2, 1, 6];
```

函数做事情。所以，函数的名字应该是一个动词。

```
**// Don't**
function usernameValidation(username) {}**// Do**
function validateUsername(username) {}
```

以**开头的**为布尔型:

```
let isValidName = validateName(‘amyjandrews’);
```

不要直接使用常量，因为随着时间的推移，你会想，“这到底是什么？”最好在使用常量之前给它们命名:

```
**// Don't** let area = 5 * 5 * 3.14;**// Do**
const PI = 3.14;
let radius = 5;
let area = PI * radius * radius;
```

对于回调函数，不要偷懒，只把参数命名为一个字符，就像 **h，j，d** (可能连你，那些名字的父亲，都不知道它们是什么意思)。长话短说，如果参数是人，传**人**；如果是书，传**书**:

```
**// Don't**
let books = [‘Learn JavaScript’, ‘Coding for Beginners’, ‘CSS the Good Parts’];books.forEach(function(b) {
  // …
});**// Do**
let books = [‘Learn JavaScript’, ‘Coding for Beginners’, ‘CSS the Good Parts’];books.filter(function(book) {
  // …
});
```

# 2.抛出信息错误

*“出现错误。”*

或者干脆:*“错误。”*

什么错误？

每次在一些应用或者网站上看到那样的错误，作为一个用户，我都很讨厌。我做错了什么？我犯了那个错误？什么错误？你不告诉我，我怎么知道我接下来要做什么？

你的用户大概和我有一样的感受。有时候他们会卸载你的 app，再也回不来了。

写一个清晰的错误信息一点也不难。

如果没有互联网连接，则:

```
showMessage(‘No internet connection! Please check your connection and try again!’);
```

如果用户忘记输入什么，那么:

```
showMessage(‘Please enter your username’);
```

更重要的是，明确的错误有助于快速调试，节省大量开发时间。

```
if (error) {
  throw new Error(‘validation.js:checkUser: special characters are now allowed’);
}
```

以上是你可以参考的错误信息格式。

# 3.尽快返回

看看下面的代码:

```
function login(username, password) {
  if (isValid(username)) {
    // Log in
  } else {
    showMessage(‘Username is not valid’);
  }
}
```

else 部分是不必要的。我们应该通过提前返回消息来删除它:

```
function login(username, password) {
  if (!isValid(username)) {
    showMessage(‘Username is not valid’);
    return;
  } // Log in
}
```

这使得你的代码更加清晰。边缘案例应放在前面，然后是需要处理更多逻辑的较长部分。

# 4.不要给一个功能太多的权力

每个功能应该只承担一个责任。不要把一个做很多事情的强大功能。

```
function validateAndLogin() {
  // Do a lot of things here
}
```

**和**不应该是函数名的一部分。**、**给一个职能增加更多的责任，从长远来看弊大于利。

以下是最好的:

```
function validate() {
  // Only validate
}function login() {
  // Only login
}
```

# 5.避免副作用

任何功能之外的事情都不关它的事。所以这个函数不应该接触它们中的任何一个。

例如:

```
var number = 3;function changeNumber(add) {
  number = 2 + add;
  return number;
}changeNumber();
```

当您调用 changeNumber 函数时，变量**的编号**将被更改为 6。这是一个真正的问题，因为有时你对全局变量的变化毫无头绪。所以，你应该在你的项目中避免这种副作用。

怎么会？通过使用[纯函数](https://medium.com/better-programming/what-is-a-pure-function-3b4af9352f6f)。

上面的例子可以改为:

```
function addThree(summand) {
  const constant = 3;
  const sum = summand + constant; return sum;
}
```

# 6.创建模块

你创建了一些函数。他们似乎做着类似的动作。比如**验证用户名**和**验证密码**。你感觉到它们可以被归入一个模块。姑且称之为**验证**模块。

```
const validateUsername = function (username) {
  // Validate username
};const validatePassword = function (password) {
  // Validate password
};Module.exports = {
  validateUsername,
  validatePassword
};const { 
  validateUsername,
  validatePassword
} = require(‘./validation’);let isUsernameValid = validateUsername(‘amyjandrews’);
```

# 7.使用代码格式插件

我的大多数项目都使用 VSCode。如果你使用的是 VSCode，为了代码美观，一定要安装更漂亮的。

这个插件将为你节省一些格式化代码的时间。多亏了它，你可以利用这段时间更加关注代码的质量。

综上所述，你应该考虑你将把项目转让给的人，“如何让他们感到高兴继续这个项目？”因为当有人把项目交给你时，这也是你所期望的。

[](https://medium.com/javascript-in-plain-english/how-to-write-effective-javascript-code-that-every-programmer-loves-to-maintain-a490b42b9b9f) [## 如何编写每个程序员都喜欢维护的有效 JavaScript 代码

### 要不要写更易维护的代码？运用坚实的原则

medium.com](https://medium.com/javascript-in-plain-english/how-to-write-effective-javascript-code-that-every-programmer-loves-to-maintain-a490b42b9b9f)