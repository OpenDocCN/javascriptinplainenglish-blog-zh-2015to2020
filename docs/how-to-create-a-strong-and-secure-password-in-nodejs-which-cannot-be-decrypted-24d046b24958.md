# 如何在 Node.js 中创建一个无法解密的强而安全的密码

> 原文：<https://javascript.plainenglish.io/how-to-create-a-strong-and-secure-password-in-nodejs-which-cannot-be-decrypted-24d046b24958?source=collection_archive---------1----------------------->

## 创建强密码的最佳方式

![](img/736af03f0a142c0f455ae0235851f0cd.png)

为了创建一个强密码，我们将使用一个非常流行的名为`bcryptjs`的 npm 库，它允许我们加密纯文本密码。

该库中使用的算法是哈希算法。

**加密密码和哈希密码的区别在于，如果我们知道解密密钥，加密的密码是可以解密的，而哈希算法不允许解密，安全性更高。**

所以让我们深入研究一下

**安装:**

要安装软件包，请从终端执行以下命令

```
npm install bcryptjs
```

为了散列密码，它提供了一个散列方法，该方法使用生成的散列密码返回一个承诺

```
const bcrypt = require("bcryptjs");
bcrypt.hash("thisismypassword", 8)
  .then(password => {
    console.log(password); // hashed password
});
```

哈希函数接受两个参数

1.明文密码
2。为了获得强密码，应该执行哈希算法的次数，作者建议的值是 **8** ，这将创建一个强密码，并且哈希它也不会花费太多时间。

*现在，您可能会想，如果无法解密，我们如何检查用户输入的密码是否与您存储在数据库中的哈希密码相同。*

为此，`bcryptjs`提供了一个 **compare** 方法，允许我们比较存储在数据库中的明文密码和散列密码。

`compare`方法接受以下参数

1.明文密码
2。要比较的哈希密码

它返回一个带有布尔结果的承诺。
如果密码匹配，结果为真，否则为假。

```
bcrypt.hash("thisismypassword", 8)
 .then(password => {
  **bcrypt.compare("thisismypassword", password)
    .then(isEqual => {
      console.log(isEqual); // true
   });**
});
```

关于这篇短文就是这样。希望你今天学到了新东西

**别忘了直接在你的收件箱** [**这里**](https://yogeshchavan.dev) **订阅我的每周时事通讯，里面有惊人的技巧、诀窍和文章。**