# ES2020(ES11)中的 JavaScript 新特性

> 原文：<https://javascript.plainenglish.io/new-javascript-features-in-es2020-c2d76acf9c5a?source=collection_archive---------0----------------------->

## 简短、有用的 JavaScript 课程——让它变得简单。

![](img/6b424bcba20ddd192d8c6cd0e1e1026f.png)

Image of two monitors with code in their screens

在本文中，我们将回顾 ES2020 的一些最新功能。

其中一些是:

*   私有类变量
*   承诺。都解决了
*   字符串.原型.匹配
*   可选链接运算符
*   动态导入
*   BigInt

注意:所有的例子都已经在 Chrome 版本 79 中测试过了

## 私有类变量

通过在变量或函数前面添加一个简单的散列符号，我们可以将它们完全保留给类内部使用。

```
class Person {
  #born = 1980
  age() { console.log(2020 - this.#born) }
}const person1 = new Person()
person1.age() // 40console.log(person1.#born)
//Uncaught SyntaxError: Private field '#born' must be declared in an     
//enclosing class
```

## 承诺。都解决了

自从 ES6 中引入了 promises，JavaScript 已经支持了两个 promise 组合子:静态方法 Promise.all 和 Promise.race。现在，ES2020 中添加了 Promise.allSettled。

不像 Promise.all 或 Promise.race 分别在任何承诺被拒绝或解决时短路，Promise.allSettled 运行所有承诺，而不考虑任何承诺的拒绝。

*   Promise.allSettled 不会短路 ES2020 中添加的
*   在 ES6 中增加了输入值被拒绝时的所有短路
*   在 ES6 中增加了输入值稳定时 Promise.race 短路

有了 Promise.allSettled，我们可以创建一个新的承诺，只有当传递给你的所有承诺都完成时，它才会回来。因此，它将为我们提供一个矩阵，其中包含每个承诺的数据。

```
const promiseOne = new Promise((resolve, reject) =>  
                       setTimeout(resolve, 3000));const promiseTwo = new Promise((resolve, reject) => 
                       setTimeout(reject, 3000));Promise.allSettled([promiseOne, promiseTwo]).then(data => console.log(data));
Promise {<pending>}//(2) [{…}, {…}]
//0: {status: "fulfilled", value: undefined}
//1: {status: "rejected", reason: undefined}
//length: 2
```

## 字符串.原型.匹配

给定一个字符串和一个正则表达式，matchAll()方法返回一个迭代器，该迭代器包含与一个字符串匹配的所有结果和正则表达式，包括[捕获组](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions/Groups_and_Ranges)。

语法:str.matchAll(regexp)

*   regexp:正则表达式对象。
*   返回值:一个[迭代器](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Iterators_and_Generators)(不是可重启的可迭代)。

使用带有全局/g 标志的 **match()** 时，捕获组被忽略:

```
const regexp = /g(ro)(up(\d?))/g;const groups = 'group1group2group3';groups.**match**(regexp);//(3) ["group1", "group2", "group3"]
//0: "group1"
//1: "group2"
//2: "group3"
```

使用 **matchAll** ，您可以访问捕获组:

```
const regexp = /g(ro)(up(\d?))/g;const someString = 'group1group2group3';const array = [...someString.**matchAll**(regexp)];array
//(3) [Array(4), Array(4), Array(4)]
//0: (4) ["group1", "ro", "up1", "1", index: 0, input: //"group1group2group3", groups: undefined]//1: (4) ["group2", "ro", "up2", "2", index: 6, input: //"group1group2group3", groups: undefined]//2: (4) ["group3", "ro", "up3", "3", index: 12, input: //"group1group2group3", groups: undefined]//length: 3
```

## 可选链接运算符

长的属性访问链很容易出错，在每一步检查属性的存在变成了一个深度嵌套的结构。

让我们看一个简单汽车对象的例子:

```
let car = {
  engine : {
    consumption: 10
  }
}
```

假设我们想获得消费。我们可以这样做:

```
let consumption = car.engine.consumption
//console.log(consumption)
//10 Ok, no problem here.
```

但是如果引擎是空的会怎么样呢？如果我们试图访问消费，我们将获得一个错误。所以，我们要先查一下物业。

```
let car = {
}
car.engine.consumption;//Uncaught TypeError: Cannot read property 'consumption' of  
//undefined//Before ES2020//Check if exists
let consumption = car.engine ? car.engine.consumption : undefined
//console.log(consumption )
//undefined/////or/////let car = {
}//Check if exists
let consumption;
if(car.engine && car.engine.consumption){
  let consumption = cat.engine.consumption
}else{
  let consumption = undefined
}console.log(consumption)
//undefined
```

这是可行的，但这样做是很清楚的:

```
let car = {
}let consumption = car.engine?.consumptionconsole.log(consumption);
//undefined
```

你也可以链可选链:

```
let car = null;
let consumption = car?.engine?.consumption
console.log(consumption);
```

例如，可选的链接作用于数组或函数调用。

设 array = null

```
//This makes sure that array exists before trying to access the //first element.
let car1 = array?.[1];
```

这是一个可选的链接操作符，简单而且非常有用！

## 动态导入

动态导入()引入了一种新的类似函数的导入形式，与静态导入相比，它释放了新的功能，例如，我们可以在延迟加载中使用它。

请考虑位于的以下内容。/greetingsModule.js:

```
//greetingsModule.jsexport hello () => console.log("Hello World!");
```

这里我们静态导入并使用。/greetings 模块. js 模块:

```
//main.jsimport * as greet from './ greetingsModule.js’;greet.hello();
//Hello World!
```

静态导入语法只能在文件的顶层使用。

相反，使用动态导入， **import(module** )表达式加载模块并返回一个承诺，该承诺解析为包含其所有导出的模块对象。**同样，你可以在代码**的任何地方调用它。

```
...if( 1 === 1){**import(’./greetingsModule.js’).then( (greet) => {
             greet.hello();
            // Hello World!
         });
}**...
```

或者您可以使用 async/await:

```
...async function load() {
    let greet = await import(’./greetingsModule.js’);
    greet.hello(); 
    // Hello!
  }...
```

静态导入和动态导入()都很有用。每一种都有其使用案例。使用静态导入，例如，对于初始依赖关系。在其他情况下，考虑使用动态导入特性按需加载依赖项。

## BigInt

BigInt 是一个新的内置对象，它提供了一种表示大于 2^(53 的整数的方法)–1。JavaScripts Number 对固定范围的数字有严格的限制，不能正确处理大整数。BigInt 的新特性解决了这个问题，这个问题在处理重要值的不同领域可能是一个相当大的问题。

```
console.log(Number.MAX_SAFE_INTEGER);
//9007199254740991const max = Number.MAX_SAFE_INTEGER;console.log(max +1);
//9007199254740992  -> Correct value!console.log(max +10);
//900719925474**1000** -> **Incorrect value**! (1001)
```

我们可以用新的 BigInt 数据类型来解决这个问题。**在数字末尾加上字母‘n’**:

```
const myBigNumber = 9007199254740991n;console.log(myBigNumber +1n);
//9007199254740992n  -> Correct value!console.log(myBigNumber +10n);
//900719925474**1001**n  -> **Correct value!**//Note:console.log(myBigNumber +10);//Error: you cannot mix BigInt and other types, use explicit //conversions.//Correct way: You have to add the letter 'n' on the end of the //number
```

更多尽在[**es 2019(ES10)**](https://medium.com/javascript-in-plain-english/javascript-es2019-es10-in-a-nutshell-cae6f7524519)中的 JavaScript 新特性

## 非常感谢你阅读我！保重！