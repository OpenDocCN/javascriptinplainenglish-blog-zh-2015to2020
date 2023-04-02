# ES6 导入和导出语法更改的完整指南

> 原文：<https://javascript.plainenglish.io/complete-guide-for-es6-import-and-export-syntax-changes-95f930882fa?source=collection_archive---------1----------------------->

## 轻松地在文件之间分发代码，而不用担心命名冲突

![](img/478edfc585a5b3db809dc23d10a7fbf9.png)

Photo by [chuttersnap](https://unsplash.com/@chuttersnap?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在 ES6 开始使用之前，我们在一个 html 文件中有多个脚本标签来导入不同的 javascript 文件

```
<script type="text/javascript" src="home.js"></script>
<script type="text/javascript" src="profile.js"></script>
<script type="text/javascript" src="user.js"></script>
```

因此，如果我们在不同的 javascript 文件中有一个同名的变量，这将产生命名冲突，并且您期望的值将不是您得到的实际值。

ES6 通过模块的概念解决了这个问题。

我们在 ES6 中编写的每个 javascript 文件都被称为一个模块，我们在每个文件中声明的变量和函数对其他文件不可用，直到我们专门从该文件导出它们并将其导入另一个文件。

所以文件中定义的函数和变量对于每个文件都是私有的，在我们导出它们之前，不能在文件外部访问它们。

有两种类型的出口

1.  **命名导出:**一个文件中可以有多个命名导出
2.  **默认导出:**一个文件中只能有一个默认导出

**命名出口:**

要将单个值导出为命名导出，我们像这样导出它

```
export const temp = "This is some dummy text";
```

我们也可以在单独的一行上写一个导出语句，而不是在变量声明之前，并在花括号中指定要导出的内容。

```
const temp1 = "This is some dummy text1";
const temp2 = "This is some dummy text2";
export { temp1, temp2 };
```

请注意，导出语法不是对象文字语法。所以在 ES6 中，为了导出某些东西，我们不能使用键值对，比如

```
// This is invalid syntax of export in ES6
export { key1: value1, key2: value2 }
```

为了导入我们作为命名导出导出的内容，我们使用以下语法

```
import { temp1, temp2 } from './filename';
```

注意，在从文件导入内容时，我们不需要添加。默认情况下文件名的 js 扩展名。

```
// import from functions.js file from current directory
import { temp1, temp2 } from './functions';// import from functions.js file from parent of current directory
import { temp1 } from '../functions';
```

请看例子并在这里玩导入导出语法[这里](https://codesandbox.io/s/hardcore-pond-q4cjx)我们使用导入导出语法计算圆的面积:[https://codesandbox.io/s/hardcore-pond-q4cjx](https://codesandbox.io/s/hardcore-pond-q4cjx)

**需要注意的一点是，导出时使用的名称必须与我们导入时使用的名称相匹配**

因此，如果您要导出为

```
// constants.js
export const PI = 3.14159;
```

进口时，我们必须使用

```
import { PI } from './constants';
```

我们不能用其他名字，比如

```
import { PiValue } from './constants'; // This will throw an error
```

但是如果我们已经有了与导出变量同名的变量，我们可以在导入时使用重命名语法

```
import { PI as PIValue } from './constants';
```

这里我们已经将 PI 重命名为 PIValue，所以我们现在不能使用 PI 变量名，我们必须使用`PIValue`变量来获得 PI 的导出值。

我们也可以在导出时使用重命名语法

```
// constants.js
const PI = 3.14159;
export { PI as PIValue };
```

然后在导入时，我们必须使用 PIValue，比如

```
import { PIValue } from './constants';
```

要将某个东西导出为命名导出，我们必须首先声明它。

```
export 'hello'; // this will result in error
export const greeting = 'hello'; // this will work
export { name: 'David' }; // This will result in error
export const object = { name: 'David' }; // This will work
```

**我们导入多个指定导出的顺序并不重要。
T3 看一下这个例子:【https://codesandbox.io/s/youthful-flower-xesus】T4**

在 components/validations.js 中，我们将验证函数导出为

```
export { isValidEmail, isValidPhone, isEmpty };
```

在`index.js`中，我们将验证函数作为

```
import { isEmpty, isValidEmail } from "./components/validations";
```

如您所见，我们可以只导入所需的导出内容，并且以任何顺序导入，因此我们不需要检查我们在另一个文件中导出的顺序。这就是命名出口的美妙之处

**默认出口:**

如前所述，一个文件中最多只能有一个默认导出。

您可以在单个文件中组合多个命名导出和一个默认导出

要声明默认导出，我们在导出关键字前面添加默认关键字，如

```
//constants.js
const name = 'David';export default name;
```

为了导入默认导出，我们不像在命名导出中那样添加花括号。

```
import name from './constants';
```

如果我们有多个命名导出和一个默认导出，如

```
// constants.js
export const PI = 3.14159;export const AGE = 30;const NAME = "David";export default NAME;
```

为了在一行中导入所有内容，我们在花括号前使用默认的导出变量

```
// NAME is default export and PI and AGE are named exports here
import NAME, { PI, AGE } from './constants';
```

**默认导出的一个特点是，我们可以在导入**时更改导出变量的名称

```
// constants.js
const AGE = 30;
export default AGE;
```

在另一个文件中，我们可以在导入时使用另一个名称

```
import myAge from ‘./constants’;console.log(myAge); // 30
```

这里，我们将默认导出变量的名称从 AGE 更改为 myAge。

这样做是因为只能有一个默认导出，所以您可以随意命名它。

**关于默认导出，另一件要注意的事情是导出默认关键字不能出现在变量声明之前，就像**

```
// constants.jsexport default const AGE = 30; // This is an error and will not work
```

所以我们必须在单独的一行中使用 export default 关键字，比如

```
// constants.jsconst AGE = 30;export default AGE;
```

但是，我们可以导出 default，而不用声明变量，如

```
//constants.jsexport default {
 name: "Billy",
 age: 40
};
```

在另一个文件中

```
import user from './constants';console.log(user.name); // Billy
console.log(user.age); // 40
```

使用以下语法，还有一种方法可以导入文件中导出的所有变量

```
import * as constants from './constants';
```

这里，我们导入 constants.js 中的所有命名和默认导出，并存储在 constants 变量中。所以，常数现在会变成一个对象。

```
// constants.jsexport const USERNAME = "David";export default {
 name: "Billy",
 age: 40
};
```

在另一个文件中，我们如下使用它

```
// test.js**import * as constants from './constants';**console.log(constants.USERNAME); // Davidconsole.log(constants.default); // { name: "Billy", age: 40 }console.log(constants.default.age); // 40
```

在这里玩默认出口:[https://codesandbox.io/s/green-hill-dj43b](https://codesandbox.io/s/green-hill-dj43b)

如果您不想将默认导出和命名导出放在不同的行上，您可以将其合并为

```
// constants.jsconst PI = 3.14159;
const AGE = 30;
const USERNAME = "David";const USER = {
 name: "Billy",
 age: 40
};export { PI, AGE, USERNAME, USER as default };
```

这里，我们将用户导出为默认导出，将其他用户导出为命名导出。
在另一个文件中，你可以用它作为

```
import USER, { PI, AGE, USERNAME } from "./constants";
```

在这里玩一下:[https://codesandbox.io/s/eloquent-northcutt-7btp1](https://codesandbox.io/s/eloquent-northcutt-7btp1)

**总结:**

*1。在 ES6 中，在一个文件中声明的数据不可被另一个文件访问，直到从该文件导出并导入另一个文件*

*2。如果我们在一个文件中有一个要导出的东西，比如类声明，我们使用默认导出，否则我们使用命名导出。我们还可以将默认导出和命名导出合并到一个文件中。*

今天到此为止。希望你今天学到了新东西。

**别忘了直接在你的收件箱** [**这里**](https://yogeshchavan.dev) **订阅我的每周时事通讯，里面有惊人的技巧、诀窍和文章。**