# ES6:休息和扩散有什么区别？

> 原文：<https://javascript.plainenglish.io/es6-spread-parameter-vs-rest-operator-5e3c924c4e1f?source=collection_archive---------0----------------------->

![](img/a352a51242de2ee8ccc769a14ead07d9.png)

# 休息参数[…休息]

它将所有剩余的元素*(因此得名 rest，如“其余的元素”)*集合成一个数组。

```
var myName = ["Marina" , "Magdy" , "Shafiq"] ;
const [firstName , ...familyName] = myName ;
console.log(firstName); // Marina ;
console.log(familyName); // [ "Magdy" , "Shafiq"] ;
```

如上面例子中的第 2 行所示，这三个点将剩余的`myName`元素收集到一个新的子数组中，这就是所谓的**析构**，它将我的数组或对象分解成更小的块。

使用 rest parameter 进行析构帮助我们将数组分解成一个可以直接调用的主参数，如 *firstName* ，并将其余的参数收集到另一个数组中，如 *familyName。*

## 我们又能在哪里找到 rest 参数呢？

如果您要调用一个函数并发送一些参数，您将在函数实现中将它们接收到 rest 参数中。

```
function myData(...args){
console.log(args) ; // ["Marina",24,"Front-End Developer"]
}
myData("Marina",24,"Front-End Developer") ;
```

如上例所示，`*myData*` 在`…args`中接收参数，这将是一个数组，包含函数调用发送的所有参数。

# 展开运算符[…展开]

它与 rest 参数相反，rest 参数将项目收集到一个数组中，spread 操作符将收集到的元素解包为单个元素。

```
var myName = ["Marina" , "Magdy" , "Shafiq"];
var newArr = [...myName ,"FrontEnd" , 24];
console.log(newArr) ; // ["Marina" , "Magdy" , "Shafiq" , "FrontEnd" , 24 ] ;
```

## 你能看出发生了什么吗？

是的，它连接在两个数组之间，并将`myName`解包到单个元素中，并且与 rest 参数不同，spread 操作符可以是第一个元素，但是 rest 参数需要是最后一个来收集其余元素。

它可以用来将一个数组复制到另一个数组中，或者将两个数组连接在一起。

***汇总***

*   Rest 参数将所有剩余的元素收集到一个数组中。
*   Spread 运算符将收集的元素(如数组)解包为单个元素。

这个 Rest 参数和 Spread 操作符，我希望你喜欢并学会了。

> “告诉我，我忘了。教我，我会记住。让我参与，我学习。”本杰明·富兰克林

> 不要只是阅读它，试一试！

快乐学习..