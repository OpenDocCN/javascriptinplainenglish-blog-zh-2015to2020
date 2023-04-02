# 节省您时间的 JavaScript 速记技巧和窍门

> 原文：<https://javascript.plainenglish.io/20-javascript-shorthand-techniques-that-will-save-your-time-f1671aab405f?source=collection_archive---------0----------------------->

![](img/606f52f6db6a9ac62a00e1a2b8f59e74.png)

速记技术可以帮助你编写优化的代码，让你用更少的代码实现你的目标。下面我们就来逐一讨论一下 JavaScript 的一些速记技巧和窍门。

# 1.声明变量

```
//Longhand 
let x; 
let y = 20; 

//Shorthand 
let x, y = 20;
```

# 2.为多个变量赋值

我们可以用数组析构在一行中给多个变量赋值。

```
//Longhand 
let a, b, c; 
a = 5; 
b = 8; 
c = 12;

//Shorthand 
let [a, b, c] = [5, 8, 12];
```

# 3.三元运算符

我们可以用三元(条件)运算符节省 5 行代码。

```
let marks = 26;

//Longhand
let result; 
if(marks >= 30){
 result = 'Pass'; 
}else{ 
 result = 'Fail'; 
}

//Shorthand 
let result = marks >= 30 ? 'Pass' : 'Fail';
```

# 4.分配默认值

我们可以使用`OR(||)`短路评估或 Nullish 合并运算符(？？)将默认值赋给变量，以防发现预期值有误。

```
//Longhand 
let imagePath; 
let path = getImagePath(); 
if(path !== null && path !== undefined && path !== '') { 
  imagePath = path; 
} else { 
  imagePath = 'default.jpg'; 
} 

//Shorthand 1
let imagePath = getImagePath() || 'default.jpg';

//Shorthand 2
let imagePath = getImagePath() ?? 'default.jpg';
```

# 5.和(&&)短路评估

如果只有当变量为真时才调用函数，那么可以使用`AND(&&)`短路作为替代。

```
//Longhand 
if (isLoggedin) {
 goToHomepage(); 
} 

//Shorthand 
isLoggedin && goToHomepage();
```

当您想要有条件地呈现一个组件时，`AND(&&)`短路简写在 **React** 中更有用。例如:

```
<div> { this.state.isLoading && <Loading /> } </div>
```

# 6.交换两个变量

为了交换两个变量，我们经常使用第三个变量。我们可以通过数组析构赋值轻松交换两个变量。

```
let x = 'Hello', y = 55; 

//Longhand 
const temp = x; 
x = y; 
y = temp; 

//Shorthand 
[x, y] = [y, x];
```

# 7.箭头功能

```
//Longhand 
function add(num1, num2) { 
   return num1 + num2; 
} 

//Shorthand 
const add = (num1, num2) => num1 + num2;
```

**引用** : [JavaScript 箭头函数](https://jscurious.com/javascript-arrow-function/)

# 8.模板文字

我们通常使用`+`操作符将字符串值和变量连接起来。使用 ES6 模板文字，我们可以用一种更简单的方式来完成。

```
//Longhand 
console.log('You got a missed call from ' + number + ' at ' + time); 

//Shorthand 
console.log(`You got a missed call from ${number} at ${time}`);
```

# 9.多行字符串

对于多行字符串，我们通常使用带有新的行转义序列(`\n`)的`+`操作符。我们可以通过使用反斜线(```)以一种更简单的方式来实现。

```
//Longhand 
console.log('JavaScript, often abbreviated as JS, is a \n' +             
'programming language that conforms to the \n' + 
'ECMAScript specification. JavaScript is high-level,\n' + 
'often just-in-time compiled, and multi-paradigm.');

//Shorthand 
console.log(`JavaScript, often abbreviated as JS, is a 
programming language that conforms to the 
ECMAScript specification. JavaScript is high-level, 
often just-in-time compiled, and multi-paradigm.`);
```

# 10.多重条件检查

对于多值匹配，我们可以将所有值放在一个数组中，并使用`indexOf()`或`includes()`方法。

```
//Longhand 
if (value === 1 || value === 'one' || value === 2 || value === 'two') { 
    // Execute some code 
} 

// Shorthand 1
if ([1, 'one', 2, 'two'].indexOf(value) >= 0) { 
   // Execute some code 
}

// Shorthand 2
if ([1, 'one', 2, 'two'].includes(value)) { 
   // Execute some code 
}
```

# 11.对象属性分配

如果变量名和对象键名相同，那么我们可以只在对象文字中提到变量名，而不是键和值。JavaScript 将自动设置与变量名相同的键，并将值赋为变量值。

```
let firstname = 'Amitav'; 
let lastname = 'Mishra'; 

//Longhand 
let obj = { firstname: firstname, lastname: lastname };

//Shorthand 
let obj = { firstname, lastname };
```

# 12.串成一个数字

有像`parseInt`和`parseFloat`这样的内置方法可以将字符串转换成数字。我们也可以通过简单地在字符串值前面提供一元运算符(+)来做到这一点。

```
//Longhand 
let total = parseInt('453'); 
let average = parseFloat('42.6'); 

//Shorthand 
let total = +'453'; 
let average = +'42.6';
```

# 13.多次重复一个字符串

要将一个字符串重复指定的次数，可以使用一个`for`循环。但是使用`repeat()`方法，我们可以在一行中完成。

```
//Longhand 
let str = ''; 
for(let i = 0; i < 5; i ++) { 
  str += 'Hello '; 
} 
console.log(str); // Hello Hello Hello Hello Hello 

// Shorthand 
'Hello '.repeat(5);
```

# 14.指数幂

我们可以用`Math.pow()`法求一个数的幂。有一个更短的语法可以用双星号(`**`)来完成。

```
// Longhand 
const power = Math.pow(4, 3); // 64 

// Shorthand 
const power = 4**3; // 64
```

# 15.双位非运算符(~~)

双位 NOT 运算符是`Math.floor()`方法的替代。

```
// Longhand 
const floor = Math.floor(6.8); // 6 

// Shorthand 
const floor = ~~6.8; // 6
```

> **由**[**Caleb**](https://medium.com/u/b6496fbb22a5?source=post_page-----f1671aab405f--------------------------------):*双非按位运算符方法仅适用于 32 位整数，即(2**31)-1 = 2147483647。所以对于任何高于 2147483647 的数字，按位运算符(~~)都会给出错误的结果，所以在这种情况下建议使用* `*Math.floor()*` *。*

# 16.查找数组中的最大值和最小值

我们可以使用 for 循环遍历数组中的每个值，找到最大值或最小值。我们还可以使用 [Array.reduce()](https://jscurious.com/a-guide-to-array-reduce-method-in-javascript/) 方法来查找数组中的最大值和最小值。

但是使用扩展操作符，我们可以在一行中完成。

```
// Shorthand 
const arr = [2, 8, 15, 4]; 
Math.max(...arr); // 15 
Math.min(...arr); // 2
```

# 17.For 循环

为了遍历一个数组，我们通常使用传统的`for`循环。我们可以利用`for...of`循环来遍历数组。要访问每个值的索引，我们可以使用`for...in`循环。

```
let arr = [10, 20, 30, 40]; 

//Longhand 
for (let i = 0; i < arr.length; i++) { 
  console.log(arr[i]); 
} 

//Shorthand 
//for of loop 
for (const val of arr) { 
  console.log(val); 
} 

//for in loop 
for (const index in arr) { 
  console.log(`index: ${index} and value: ${arr[index]}`); 
}
```

我们也可以使用`for...in`循环遍历对象属性。

```
let obj = { x: 20, y: 50 };

for (const key in obj) { 
  console.log(obj[key]); 
}
```

**引用**:[JavaScript 中遍历对象和数组的不同方式](https://jscurious.com/different-ways-to-iterate-through-objects-and-arrays-in-javascript/)

# 18.数组的合并

```
let arr1 = [20, 30]; 

// Longhand 
let arr2 = arr1.concat([60, 80]); 
// [20, 30, 60, 80] 

// Shorthand 
let arr2 = [...arr1, 60, 80]; 
// [20, 30, 60, 80]
```

# 19.多级对象的深度克隆

要深度克隆一个多级对象，我们可以遍历每个属性并检查当前属性是否包含一个对象。如果是，则通过传递当前属性值(即嵌套对象)对同一个函数进行递归调用。

如果我们的对象不包含 functions、undefined、NaN 或 Date 作为值，我们也可以通过使用`JSON.stringify()`和`JSON.parse()`来实现。

如果我们有一个单级对象，即没有嵌套对象，那么我们也可以使用 spread 操作符进行深度克隆。

```
let obj = {x: 20, y: {z: 30}}; 

// Longhand 
const makeDeepClone = (obj) => { 
  let newObject = {}; 
  Object.keys(obj).map(key => { 
    if(typeof obj[key] === 'object'){ 
      newObject[key] = makeDeepClone(obj[key]); 
    } else { 
      newObject[key] = obj[key]; 
    } 
  }); 
 return newObject; 
} 
const cloneObj = makeDeepClone(obj); 

// Shorthand 
const cloneObj = JSON.parse(JSON.stringify(obj));

// Shorthand for single level object
let obj = {x: 20, y: 'hello'};
const cloneObj = {...obj};
```

> **来自注释的改进:** *如果您的对象属性包含* `*function*` *、* `*undefined*` *或* `*NaN*` *作为值，则速记技巧(* `*JSON.parse(JSON.stringify(obj))*` *)不起作用。因为当 JSON.stringify 对象时，包含* `*function*` *、* `*undefined*` *或* `*NaN*` *作为值的属性会从对象中移除。*
> 
> *所以当你的对象只包含字符串和数字的时候就用* `*JSON.parse(JSON.stringify(obj))*` *。*

参考: [JSON.parse()和 JSON.stringify()](https://jscurious.com/difference-between-json-parse-and-json-stringify/)

# 20.从字符串中获取字符

```
let str = 'jscurious.com';

// Longhand 
str.charAt(2); // c

// Shorthand 
str[2]; // c
```

# 21.展平嵌套数组

我们可以使用`Array.flat(depth)`方法来展平嵌套数组。

```
const arr = [1, [2, 3, [4, [5]], 6], 7];
console.log(arr.flat(1)); // [1, 2, 3, [4, [5]], 6, 7]
console.log(arr.flat(2)); // [1, 2, 3, 4, [5], 6, 7]
```

如果不确定深度，使用`flat(Infinity)`展平任意深度的数组。

```
console.log(arr.flat(Infinity)); // [1, 2, 3, 4, 5, 6, 7]
```

> 注意:这些速记技巧中的一些似乎与项目中的使用无关，但是知道一些额外的技巧也不错。编码快乐！

# 你可能也喜欢

*   [使用 JavaScript 中的通知 API 发送推送通知](https://jscurious.com/the-notification-api-in-javascript/)
*   [用 JavaScript 中的 HTMLAudioElement API 播放音频](https://jscurious.com/play-audio-with-htmlaudioelement-api-in-javascript/)
*   [JavaScript 中的地图，当它是比对象更好的选择时](https://jscurious.com/map-in-javascript-and-how-it-is-better-than-object/)
*   [JavaScript 设置对象存储唯一值](https://jscurious.com/javascript-set-object-to-store-unique-values/)
*   [JavaScript 中的生成器函数](https://jscurious.com/generator-functions-in-javascript/)
*   [JavaScript 中承诺的简要指南](https://jscurious.com/a-brief-guide-to-promises-in-javascript/)
*   [JavaScript 获取 API 以发出 HTTP 请求](https://jscurious.com/javascript-fetch-api-to-make-http-requests/)
*   [JavaScript 中检查对象中是否存在属性的不同方法](https://jscurious.com/different-ways-to-check-if-a-key-exists-in-an-object/)
*   [JavaScript 中的震动 API](https://jscurious.com/the-vibration-api-in-javascript/)
*   [JavaScript 中的 URLSearchParams API](https://jscurious.com/the-urlsearchparams-api/)

*感谢您抽出时间* ☺️
欲了解更多网络开发博客，请访问[jscurious.com](http://jscurious.com/)
[阿米塔夫·米什拉](https://jscurious.com/amitav-mishra)