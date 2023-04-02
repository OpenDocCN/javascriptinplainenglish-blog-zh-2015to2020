# Node.js 中使用 Mongoose 进行数据验证

> 原文：<https://javascript.plainenglish.io/store-clean-data-by-validating-models-with-mongoose-f6453dbdbff9?source=collection_archive---------1----------------------->

## 如何在用 mongoose 库将数据保存到 MongoDB 之前验证数据

![](img/8aca76816069fc5518aff7f250aa3124.png)

Photo by [fabio](https://unsplash.com/@fabioha?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 1.介绍

你好，我的开发者朋友们！！！！

创建函数和过程将数据保存到数据库中是软件开发人员的日常任务，但是我们如何确定实际存储的数据具有我们想要的形式呢？？？

> 保存干净和正确格式化的数据提高了整体数据质量，并且我们的应用程序更加安全。

在这个故事中，我们将学习如何在用 mongoose 库将数据存储到 MongoDB 之前对其进行验证。

Mongoose 是 MongoDB 和 node.js 的一个很棒的对象建模工具，它受到数百万开发者的信任，在 npm 有近 80 万次弱下载，在 GitHub 有 20.4k 星。

# 2.关于 Mongoose 中的验证

Mongoose 在模式(模型)上添加验证规则，有两种类型的规则:

1.  内置规则
2.  自定义规则

验证是一个中间件，Mongoose 在将模型保存到数据库之前运行验证。

# 3.内置验证规则

mongose 给了我们一套有用的内置验证规则，比如:

1.  验证器检查属性是否不为空。
2.  [数字](https://mongoosejs.com/docs/api.html#schema-number-js)有`min`和`max`验证器。
3.  [字符串](https://mongoosejs.com/docs/api.html#schema-string-js)有`enum`、`match`、`minlength`和`maxlength`验证器。

让我们创建一个新的 node.js 项目

```
mkdir validateModelMongoose
npm init -y
npm i mongoose
```

为我们的模型创建一个名为 **product.js** 的新文件

```
const mongoose = require("mongoose");// create product schemalet product = mongoose.Schema({name: {type: String,required: [true, 'Product name required']},category: {type: String,required: [true, 'Category required'],enum: ['Keyboard', 'Computer']},code: {type: String,required: [true, 'Code required'],minlength:[5,'Minimun code length 5 characters']},quantity: {type: Number,required: [true, 'Quantity required'],min: [0, 'Minimun quantity is zero']}});module.exports = mongoose.model("product", product);
```

在上面的代码中，我们创建了一个带有一些内置验证器的产品模型。

让我们解释一下内置验证器是怎么回事

**必需的验证器**接受一个包含两个元素的数组，首先是一个布尔变量，如果验证失败，返回一条消息。

```
required: [true, 'Product name required']
```

**枚举验证器**获取一个包含项目的数组，以检查该属性是否等于给定的数组项目之一。

```
enum: ['Keyboard', 'Computer']
```

**最小验证器**接受一个包含两个元素的数组，第一个是一个设置最小值的数字，第二个是一条如果验证失败则返回验证结果的消息。

**Max、** **minlength** 和 **maxlegth** 的工作原理与最小值验证器相同。

```
min: [0, 'Minimun quantity is zero']
```

创建一个 index.js 文件并粘贴以下代码

```
'use strict'var mongoose = require("mongoose");let PRODUCT = require("./product");let db = mongoose.connection;//connect with local mongodbmongoose.connect("mongodb://localhost:27017/products", {useNewUrlParser: true,useUnifiedTopology: true});
//create a new productvar product = new PRODUCT();product.name = '';product.category = 'Computers';product.code = '123';product.quantity = '-5';//Save Product to Databaseproduct.save(function (error,document) {//check for errorslet errors = getErrors(error);//Send Errors to browserconsole.log(errors);});function getErrors(error) {let errorArray = [];if (error) {if (error.errors['category']) {console.log(error.errors['category'].message)errorArray.push('category');}if (error.errors['name']) {console.log(error.errors['name'].message)errorArray.push('name');}if (error.errors['code']) {console.log(error.errors['code'].message)errorArray.push('code');}if (error.errors['quantity']) {console.log(error.errors['quantity'].message)errorArray.push('quantity');}} else {console.log('No Errors Product Saved Succefully')}return errorArray;};
```

运行代码并检查错误

```
node index.js
```

示例输出

```
`Computers` is not a valid enum value for path `category`.
Product name required
Minimun code length 5 characters
Minimun quantity is zero
[ 'category', 'name', 'code', 'quantity' ]
```

上面的代码向我们展示了 mongoose 在将验证中间件保存到数据库之前调用了它，并向我们抛出了已经验证的错误。

# 4.自定义同步验证器

在这一节中，我们将学习如何创建我们的定制同步验证器。同步操作阻塞了事件队列，这就是为什么我们必须明智地使用它。

产品代码通常有一种格式，如 XXXX-XXXX-XXXX，为了检查产品代码是否有正确的格式，我们将创建一个带有 **regex** 的自定义验证器来检查格式。

转到您的模型，用以下代码更改您的**代码**属性

```
code: {type: String,required: [true, 'Code required'],//sync validationvalidate: {validator: function (v) {//regex product code must have XXXX-XXXX-XXXX format//return true to pass the validation//return false to fail the validationreturn (/\d{4}-\d{4}-\d{4}/.test(v));},
//message to return if validation failsmessage: props => `${props.value} is not a valid code format!`},required: [true, 'Code required']}
```

自定义验证器位于 **code.validate** 处，首先接受两个属性:一个**验证器** **函数**，返回 true(验证成功)或 false(验证失败)，一个**消息**，如果验证失败，则返回。

运行您的应用

```
node index
// example output
123456 is not a valid code format!
```

# 5.自定义异步验证器

最后，我们将看到如何实现异步定制验证器。我们将把**同步**自定义验证改为**异步。**

要添加异步验证规则，我们必须在**code . validate . validator**实现一个函数，该函数将返回一个**承诺**和**解析**真(验证成功)或假(验证失败)

转到您的模型，用以下代码更改您的**代码**属性

```
code: {type: String,required: [true, 'Code required'],//async validationvalidate: {validator: function (v) {return new Promise(function (resolve, reject) {//regex product code must have XXXX-XXXX-XXXX format//resolve(true) pass valitation//resolve(false) fail valitationresolve(/\d{4}-\d{4}-\d{4}/.test(v));});},message: props => `${props.value} is not a valid code format!`}}
```

上面的代码显示了如何创建一个异步定制验证器。

运行您的应用

```
node index.js
//example output
123 is not a valid code format!
[ 'code' ]
```

# 6.结论

Mongoose 为我们提供了在保存到 MongoDB 之前验证模型的简单、干净和快速的方法，因此值得使用这些功能。

# 参考

*   [猫鼬](https://mongoosejs.com)
*   [node.js](https://nodejs.org/en/)

# 感谢你阅读我的故事

如有任何想法或变化，请随时给我发邮件或评论。

[](https://medium.com/javascript-in-plain-english/node-js-bcrypt-vs-bcryptjs-benchmark-69a9e8254cc2) [## Node.js Bcrypt vs BcryptJS 基准测试

### Bcrypt 是最流行、最安全的单向密码散列函数之一。

medium.com](https://medium.com/javascript-in-plain-english/node-js-bcrypt-vs-bcryptjs-benchmark-69a9e8254cc2) [](https://medium.com/swlh/master-ejs-template-engine-with-node-js-and-expressjs-979cc22b69be) [## 具有 Node.js 和 Expressjs 的主 EJS 模板引擎

### EJS 是 node.js 和 expressjs 最受欢迎的模板视图引擎之一，在 github 上有 4.2k 星级，超过 550 万…

medium.com](https://medium.com/swlh/master-ejs-template-engine-with-node-js-and-expressjs-979cc22b69be)