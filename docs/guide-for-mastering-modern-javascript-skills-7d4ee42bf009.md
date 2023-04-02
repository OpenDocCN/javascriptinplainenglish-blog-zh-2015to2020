# 掌握现代 JavaScript 技能指南

> 原文：<https://javascript.plainenglish.io/guide-for-mastering-modern-javascript-skills-7d4ee42bf009?source=collection_archive---------1----------------------->

## 成为现代 JavaScript 专家，更擅长 React、Angular、Vue

![](img/afd9be8aab6dff4f0c3217dfbf13caae.png)

Photo by [Artem Sapegin](https://unsplash.com/@sapegin?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在当今不断变化的世界中，JavaScript 出现了许多新内容和更新，这对提高代码质量非常有用。

无论是为了获得一份高薪工作，还是为了跟上最新趋势、提高代码质量，或者是为了维持你目前的工作，了解这些事情都非常重要。

JavaScript 中有许多最新的添加，如 **Nullish 合并运算符**、**可选链接**、**承诺**、**异步/等待**、 **ES6 析构**，以及许多其他非常有用的特性。

让我们探索一些你应该知道的现代 JavaScript 技巧。

# 让和 const

在 ES6 出现之前，JavaScript 使用的是`var`关键字，所以 JavaScript 只有一个函数和全局范围。没有块级范围。

随着`let`和`const`的加入，JavaScript 增加了块范围。

**使用 let:**

当我们使用`let`关键字声明一个变量时，我们**可以稍后给**分配一个新值，但是我们**不能用相同的名字重新声明**。

```
// ES5 Code
var value = 10;
console.log(value); // 10var value = "hello";
console.log(value); // hellovar value = 30;
console.log(value); // 30
```

从上面可以看出，我们已经多次使用关键字`var`重新声明了变量`value`。

在 ES6 之前，我们能够重新声明一个之前已经声明过的变量，这个变量没有任何有意义的用途，反而会引起混乱。

如果我们已经在其他地方声明了一个同名的变量，而我们在不知道我们已经有了这个变量的情况下重新声明了它，那么我们可能会覆盖变量值，导致一些难以调试的问题。

所以当使用`let`关键字时，当你试图用相同的名字重新声明变量时，你会得到一个错误，这是一件好事。

```
// ES6 Code
let value = 10;
console.log(value); // 10let value = "hello"; // Uncaught SyntaxError: Identifier 'value' has already been declared
```

但是，下面的代码是有效的

```
// ES6 Code
let value = 10;
console.log(value); // 10value = "hello";
console.log(value); // hello
```

我们在上面的代码中没有得到错误，因为我们**给`value`变量**重新赋值，但是**没有再次声明** `value`。

现在，看一下下面的代码:

```
// ES5 Code
var isValid = true;
if(isValid) {
  var number = 10;
  console.log('inside:', number); // inside: 10
}
console.log('outside:', number); // outside: 10
```

正如你在上面的代码中看到的，当我们用`var`关键字声明一个变量时，它在`if`块之外也是可用的。

```
// ES6 Code
let isValid = true;
if(isValid) {
  let number = 10;
  console.log('inside:', number); // inside: 10
}console.log('outside:', number); // Uncaught ReferenceError: number is not defined
```

正如你在上面的代码中看到的，使用`let`关键字声明的`number`变量只能在 if 块内部访问，而在块外部是不可用的，所以当我们试图在 if 块外部访问它时，我们得到了一个引用错误。

但是如果在 if 块之外有一个`number`变量，那么它将如下所示工作:

```
// ES6 Code
let isValid = true;
let number = 20;if(isValid) {
  let number = 10;
  console.log('inside:', number); // inside: 10
}console.log('outside:', number); // outside: 20
```

这里，我们在一个单独的范围内有两个`number`变量。所以在 if 块之外，`number`的值将是 20。

看看下面的代码:

```
// ES5 Code
for(var i = 0; i < 10; i++){
 console.log(i);
}
console.log('outside:', i); // 10
```

当使用`var`关键字时，`i`甚至在`for`循环之外也是可用的。

```
// ES6 Code
for(let i = 0; i < 10; i++){
 console.log(i);
}console.log('outside:', i); // Uncaught ReferenceError: i is not defined
```

但是当使用`let`关键字时，在循环之外是不可用的。

因此，从上面的代码示例中可以看出，使用`let`关键字使得变量只能在该块内部使用，而在块外部是不可访问的。

我们也可以通过一对像这样的花括号创建一个块:

```
let i = 10;
{
 let i = 20;
 console.log('inside:', i); // inside: 20
 i = 30;
 console.log('i again:', i); // i again: 30
}console.log('outside:', i); // outside: 10
```

如果你还记得，我说过我们不能在同一个块中重新声明一个基于`let`的变量，但是我们可以在另一个块中重新声明它。从上面的代码中可以看出，我们已经重新声明了`i`，并在块中为`20`分配了一个新值，一旦声明，该变量值将只在该块中可用。

在块外，当我们打印该变量时，我们得到的是`10`而不是之前分配的值`30`，因为在块外，内部的`i`变量不存在。

如果我们没有在外面声明变量`i`，那么我们将得到一个错误，如下面的代码所示:

```
{
 let i = 20;
 console.log('inside:', i); // inside: 20
 i = 30;
 console.log('i again:', i); // i again: 30
}console.log('outside:', i); // Uncaught ReferenceError: i is not defined
```

**使用常量:**

`const`关键字与块范围功能中的`let`关键字完全相同。所以让我们来看看它们之间有什么不同。

当我们声明一个变量为`const`时，它被认为是一个常量变量，其值永远不会改变。

在`let`的例子中，我们可以像这样给这个变量赋值:

```
let number = 10;
number = 20;console.log(number); // 20
```

但是我们不能这样做，以防`const`

```
const number = 10;
number = 20; // Uncaught TypeError: Assignment to constant variable.
```

我们甚至不能将重新声明为`const`变量。

```
const number = 20;
console.log(number); // 20const number = 10; // Uncaught SyntaxError: Identifier 'number' has already been declared
```

现在，看一下下面的代码:

```
const arr = [1, 2, 3, 4];arr.push(5);console.log(arr); // [1, 2, 3, 4, 5]
```

我们说过`const`变量是常量，其值永远不会改变，但我们已经改变了上面的常量数组。那么这难道不矛盾吗？

> *不是。在 JavaScript 中数组是引用类型而不是原始类型*

所以实际存储在`arr`中的不是实际的数组，而是实际数组存储的内存位置的引用(地址)。

所以通过做`arr.push(5);`，我们实际上并没有改变`arr`指向的引用，而是改变了存储在那个引用中的值。

对象也是如此:

```
const obj = {
 name: 'David',
 age: 30
};obj.age = 40;console.log(obj); // { name: 'David', age: 40 }
```

这里，我们也没有改变`obj`所指向的引用，但是我们改变了存储在那个引用中的值。
所以上面的代码可以工作，但是下面的代码不能工作。

```
const obj = { name: 'David', age: 30 };
const obj1 = { name: 'Mike', age: 40 };
obj = obj1; // Uncaught TypeError: Assignment to constant variable.
```

上面的代码不起作用，因为我们试图改变`const`变量指向的引用。

> *所以在使用* `*const*` *时要记住的关键点是，当我们使用* `*const*` *将一个变量声明为常量时，我们不能重新定义也不能重新分配那个变量，但是如果变量是引用类型，我们可以改变存储在那个位置的值。*

所以下面的代码是无效的，因为我们要给它重新赋值。

```
const arr = [1, 2, 3, 4];
arr = [10, 20, 30]; // Uncaught TypeError: Assignment to constant variable.
```

但是请注意，我们可以改变数组内部的值，如前所述。

下面重新定义`const`变量的代码也是无效的。

```
const name = "David";
const name = "Raj"; // Uncaught SyntaxError: Identifier 'name' has already been declared
```

# 结论

*   关键字`let`和`const`在 JavaScript 中增加了块范围。
*   当我们声明一个变量为`let`时，我们不能`re-define`或`re-declare`另一个变量在同一个作用域(函数或块作用域)中有相同的名字，但是我们可以`re-assign`给它赋值。
*   当我们将一个变量声明为`const`时，我们不能在同一作用域(函数或块作用域)中`re-define`或`re-declare`另一个具有相同名称的`const`变量，但是如果变量是数组或对象之类的引用类型，我们可以更改存储在该变量中的值。

# ES6 导入和导出语法

在 ES6 开始使用之前，我们在一个 HTML 文件中有多个`script`标签来导入不同的 javascript 文件，如下所示:

```
<script type="text/javascript" src="home.js"></script>
<script type="text/javascript" src="profile.js"></script>
<script type="text/javascript" src="user.js"></script>
```

因此，如果我们在不同的 javascript 文件中有一个同名的变量，这将产生命名冲突，并且您期望的值将不是您得到的实际值。

ES6 通过模块的概念解决了这个问题。

我们在 ES6 中编写的每个 javascript 文件都被称为一个模块，我们在每个文件中声明的变量和函数对其他文件不可用，直到我们专门从该文件导出它们并将其导入另一个文件。

所以文件中定义的函数和变量对于每个文件都是私有的，在我们导出它们之前，不能在文件外部访问它们。

有两种类型的导出:

*   命名导出:一个文件中可以有多个命名导出
*   默认导出:一个文件中只能有一个默认导出

# 指定出口

要将单个值导出为命名导出，我们可以像这样导出它:

```
export const temp = "This is some dummy text";
```

如果我们有多个要导出的东西，我们可以在单独的行上写一个导出语句，而不是在变量声明之前，并在花括号中指定要导出的东西。

```
const temp1 = "This is some dummy text1";
const temp2 = "This is some dummy text2";
export { temp1, temp2 };
```

请注意，导出语法不是对象文字语法。所以在 ES6 中，为了导出某些东西，我们不能像这样使用键值对:

```
// This is invalid syntax of export in ES6
export { key1: value1, key2: value2 }
```

为了导入我们作为命名导出导出的内容，我们使用以下语法:

```
import { temp1, temp2 } from './filename';
```

注意，当从文件中导入内容时，我们不需要在文件名中添加扩展名`.js`,因为它是默认的。

```
// import from functions.js file from current directory 
import { temp1, temp2 } from './functions';// import from functions.js file from parent of current directory
import { temp1 } from '../functions';
```

Codesandbox 演示:[https://codesandbox.io/s/hardcore-pond-q4cjx](https://codesandbox.io/s/hardcore-pond-q4cjx)

**需要注意的一点是，导出时使用的名称必须与我们导入时使用的名称相匹配。**

因此，如果您导出为:

```
// constants.js
export const PI = 3.14159;
```

然后在导入时，我们必须使用与导出时相同的名称

```
import { PI } from './constants';
```

我们不能像这样使用任何其他名称:

```
import { PiValue } from './constants'; // This will throw an error
```

但是，如果我们已经有了与导出变量同名的变量，我们可以在导入时使用重命名语法，如下所示:

```
import { PI as PIValue } from './constants';
```

这里我们已经将`PI`重命名为`PIValue`，所以我们现在不能使用`PI`变量名，我们必须使用`PIValue`变量来获得`PI`的输出值。

我们也可以在导出时使用重命名语法:

```
// constants.js
const PI = 3.14159; export { PI as PIValue };
```

然后在导入时，我们必须像这样使用`PIValue`:

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

**我们导入多个指定导出的顺序并不重要。**

看看下面的`validations.js`文件。

```
// utils/validations.jsconst isValidEmail = function(email) {
if (/^[^@ ]+@[^@ ]+\.[^@ \.]{2,}$/.test(email)) {
    return "email is valid";
  } else {
    return "email is invalid";
  }
};const isValidPhone = function(phone) {
if (/^[\\(]\d{3}[\\)]\s\d{3}-\d{4}$/.test(phone)) {
    return "phone number is valid";
  } else {
    return "phone number is invalid";
  }
};function isEmpty(value) { if (/^\s*$/.test(value)) {
    return "string is empty or contains only spaces";
  } else {
    return "string is not empty and does not contain
spaces";
  } 
}export { isValidEmail, isValidPhone, isEmpty };
```

在`index.js`中，我们使用这些函数，如下所示:

```
// index.js
import { isEmpty, isValidEmail } from "./utils/validations";console.log("isEmpty:", isEmpty("abcd")); // isEmpty: string is not empty and does not contain spacesconsole.log("isValidEmail:", isValidEmail("abc@11gmail.com")); // isValidEmail: email is validconsole.log("isValidEmail:", isValidEmail("ab@c@11gmail.com")); // isValidEmail: email is invalid
```

Codesandbox 演示:[https://codesandbox.io/s/youthful-flower-xesus](https://codesandbox.io/s/youthful-flower-xesus)

如您所见，我们可以只导入所需的导出内容，并且以任何顺序导入，因此我们不需要检查我们在另一个文件中导出的顺序。这就是命名出口的妙处。

# 默认导出

如前所述，一个文件中最多只能有一个默认导出。

但是，您可以将多个命名导出和一个默认导出合并到一个文件中。

要声明默认导出，我们在导出关键字前添加默认关键字，如下所示:

```
//constants.js
const name = 'David'; 
export default name;
```

为了导入默认导出，我们不像在命名导出中那样添加花括号，如下所示:

```
import name from './constants';
```

如果我们有多个命名导出和一个默认导出，如下所示:

```
// constants.js
export const PI = 3.14159; 
export const AGE = 30;
const NAME = "David";
export default NAME;
```

然后，要在一行中导入所有内容，我们只需要在花括号前使用默认的导出变量。

```
// NAME is default export and PI and AGE are named exports here
import NAME, { PI, AGE } from './constants';
```

**默认导出的一个特点是我们可以在导入时更改导出变量的名称:**

```
// constants.js
const AGE = 30;
export default AGE;
```

在另一个文件中，我们可以在导入时使用另一个名称

```
import myAge from ‘./constants’; console.log(myAge); // 30
```

这里，我们将默认导出变量的名称从`AGE`更改为`myAge`。

这样做是因为只能有一个默认导出，所以您可以随意命名它。

**关于默认导出，另一件要注意的事情是导出默认关键字不能出现在变量声明之前，就像这样:**

```
// constants.js
export default const AGE = 30; // This is an error and will not work
```

因此，我们必须在单独的一行中使用 export default 关键字，如下所示:

```
// constants.js const AGE = 30; 
export default AGE;
```

但是，我们可以导出 default，而不用像这样声明变量:

```
//constants.js
export default {
 name: "Billy",
 age: 40
};
```

在另一个文件中这样使用它:

```
import user from './constants';
console.log(user.name); // Billy 
console.log(user.age); // 40
```

使用以下语法，还有一种方法可以导入文件中导出的所有变量:

```
import * as constants from './constants';
```

在这里，我们导入所有在`constants.js`中的命名和默认导出，并存储在`constants`变量中。所以，`constants`就成了现在的一个对象。

```
// constants.js
export const USERNAME = "David";
export default {
 name: "Billy",
 age: 40
};
```

在另一个文件中，我们如下使用它:

```
// test.js
import * as constants from './constants';
console.log(constants.USERNAME); // David
console.log(constants.default); // { name: "Billy", age: 40 }
console.log(constants.default.age); // 40
```

Codesandbox 演示:[https://codesandbox.io/s/green-hill-dj43b](https://codesandbox.io/s/green-hill-dj43b)

如果您不想在单独的行上导出默认和命名的
导出，您可以如下所示合并它:

```
// constants.js
const PI = 3.14159; const AGE = 30;
const USERNAME = "David";
const USER = {
 name: "Billy",
 age: 40 
};export { PI, AGE, USERNAME, USER as default };
```

这里我们将`USER`导出为默认导出，其他导出为命名导出。

在另一个文件中，您可以这样使用它:

```
import USER, { PI, AGE, USERNAME } from "./constants";
```

Codesandbox 演示:[https://codesandbox.io/s/eloquent-northcutt-7btp1](https://codesandbox.io/s/eloquent-northcutt-7btp1)

# 结论

1.  在 ES6 中，在一个文件中声明的数据不可被另一个文件访问，直到从该文件导出并导入到另一个文件中。
2.  如果我们在一个文件中有一个要导出的东西，比如类声明，我们使用默认导出，否则我们使用命名导出。我们还可以将默认导出和命名导出合并到一个文件中。

# 默认参数

ES6 增加了一个非常有用的特性，在定义函数时提供默认参数。

假设，我们有一个应用程序，一旦用户登录到系统，我们向他们显示一条欢迎消息，如下所示:

```
function showMessage(firstName) {
  return "Welcome back, " + firstName;
}
console.log(showMessage('John')); // Welcome back, John
```

但是，如果我们的数据库中没有用户名，因为它是注册时的可选字段，该怎么办呢？然后我们可以在用户登录后向用户显示`Welcome Guest`消息。

所以我们首先需要检查是否提供了`firstName`,然后显示相应的消息。在 ES6 之前，我们必须编写这样的代码:

```
function showMessage(firstName) {
  if(firstName) {
    return "Welcome back, " + firstName;
  } else {
    return "Welcome back, Guest";
  }
}console.log(showMessage('John')); // Welcome back, John 
console.log(showMessage()); // Welcome back, Guest
```

但是现在在 ES6 中使用默认的函数参数，我们可以编写如下所示的代码:

```
function showMessage(firstName = 'Guest') {
   return "Welcome back, " + firstName;
}console.log(showMessage('John')); // Welcome back, John 
console.log(showMessage()); // Welcome back, Guest
```

**我们可以将任何值作为默认值赋给函数参数。**

```
function display(a = 10, b = 20, c = b) { 
 console.log(a, b, c);
}display(); // 10 20 20
display(40); // 40 20 20
display(1, 70); // 1 70 70
display(1, 30, 70); // 1 30 70
```

如您所见，我们为 a 和 b 函数参数分配了唯一的值，但对于 c，我们分配的是 b 的值。因此，如果在调用函数时没有为 c 提供特定的值，那么我们为 b 提供的任何值也会被分配给 c。

在上面的代码中，我们没有为函数提供所有的参数。所以上面的函数调用将和下面的一样:

```
display(); // is same as display(undefined, undefined, undefined)
display(40); // is same as display(40, undefined, undefined)
display(1, 70); // is same as display(1, 70, undefined)
```

因此，如果传递的参数是`undefined`，那么相应的参数将使用默认值。

**我们也可以将复杂值或计算值指定为默认值。**

```
const defaultUser = {
  name: 'Jane',
  location: 'NY',
  job: 'Software Developer'
};const display = (user = defaultUser, age = 60 / 2 ) => { 
 console.log(user, age);
};
display();/* output{
  name: 'Jane',
  location: 'NY',
  job: 'Software Developer'
} 30 */
```

现在，看看下面的 ES5 代码:

```
// ES5 Code
function getUsers(page, results, gender, nationality) {
  var params = "";
  if(page === 0 || page) {
   params += `page=${page}&`; 
  }
  if(results) {
   params += `results=${results}&`;
  }
  if(gender) {
   params += `gender=${gender}&`;
  }
  if(nationality) {
   params += `nationality=${nationality}`;
  } fetch('https://randomuser.me/api/?' + params) 
   .then(function(response) {
     return response.json(); 
   })
   .then(function(result) { 
    console.log(result);
   }) 
   .catch(function(error) {
     console.log('error', error); 
   }); 
}getUsers(0, 10, 'male', 'us');
```

在这段代码中，我们通过在`getUsers`函数中传递各种可选参数，对[随机用户](https://randomuser.me/) API 进行 API 调用。

因此，在进行 API 调用之前，我们已经添加了各种 if 条件来检查参数是否被添加，并且基于此，我们正在像这样构造查询字符串:`https://randomuser.me/api/? page=0&results=10&gender=male&nationality=us`

但是，我们可以在定义函数参数时使用默认参数，而不是添加这么多 if 条件，如下所示:

```
function getUsers(page = 0, results = 10, gender = 'male',nationality = 'us') {
 fetch(`https://randomuser.me/api/?page=${page}&results=${results}&gender=${gender}&nationality=${nationality}`)
 .then(function(response) { 
  return response.json();
 }) 
 .then(function(result) {
   console.log(result); 
 })
 .catch(function(error) { 
  console.log('error', error);
  }); 
}getUsers();
```

正如你所看到的，我们已经大大简化了代码。因此，当我们不向`getUsers`函数提供任何参数时，它将采用默认值，我们也可以像这样提供自己的值:

```
getUsers(1, 20, 'female', 'gb');
```

因此它将覆盖函数的默认参数。

# 空不等于未定义

但是你需要注意的一件事是`null`和`undefined`在定义默认参数时是两回事。

看看下面的代码:

```
function display(name = 'David', age = 35, location = 'NY'){
 console.log(name, age, location); 
}display('David', 35); // David 35 NY
display('David', 35, undefined); // David 35 NY
```

由于我们没有在第一次调用中提供第三个参数来显示，默认情况下它将是`undefined`，因此 location 的默认值将在两个函数调用中使用。但是下面的函数调用并不相等。

```
display('David', 35, undefined); // David 35 NY
display('David', 35, null); // David 35 null
```

当我们将`null`作为参数传递时，我们特别告诉将`null`的值赋给与`undefined`不同的`location`参数，因此它不会采用`NY`的默认值。

# 数组.原型.包含

ES7 增加了一个`includes`方法，用于检查数组中是否存在元素，并返回一个布尔值`true`或`false`。

```
// ES5 Codeconst numbers = ["one", "two", "three", "four"];console.log(numbers.indexOf("one") > -1); // true
console.log(numbers.indexOf("five") > -1); // false
```

使用 Array `includes`方法的相同代码可以写成如下所示:

```
// ES7 Codeconst numbers = ["one", "two", "three", "four"];console.log(numbers.includes("one")); // true
console.log(numbers.includes("five")); // false
```

所以使用 Array `includes`方法使得代码简短易懂。

在比较不同的值时,`includes`方法也很方便。

看看下面的代码:

```
const day = "monday";if(day === "monday" || day === "tuesday" || day === "wednesday") {
  // do something
}
```

上述使用`includes`方法的代码可以简化如下:

```
const day = "monday";if(["monday", "tuesday", "wednesday"].includes(day)) {
  // do something
}
```

所以在检查数组中的值时，`includes`方法非常方便。

# 结束点

**这只是** [**掌握现代 JavaScript**](https://yogeshchavan1.podia.com/mastering-modern-javascript?coupon=LA1HR55) **指南中的一些内容的预览。**

《T21 掌握现代 JavaScript》这本书涵盖了成为现代 JavaScript 技能专家所需要知道的一切。

**通过一次性购买，您将在每次新发布的 ESNext 中免费获得该书的更新版本。**

这本书包含了所有的概念，这些概念是学习 React 的先决条件，也是更好地使用 React 所必需的。

这是你面对任何 JavaScript/React 面试所需要的唯一指南，在这些面试中，ES6+特性是成功面试的必备要素。

另外，查看我的免费[React 路由器简介](https://yogeshchavan1.podia.com/react-router-introduction)课程，从头开始学习 React 路由器。

想要了解关于 JavaScript、React、Node.js 的最新常规内容吗？在 LinkedIn 上关注我。

不要忘记订阅我的每周简讯，里面有惊人的技巧、窍门和文章，直接在这里的收件箱里。