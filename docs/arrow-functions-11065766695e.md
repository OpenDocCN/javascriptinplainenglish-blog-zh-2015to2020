# 箭头功能= >

> 原文：<https://javascript.plainenglish.io/arrow-functions-11065766695e?source=collection_archive---------0----------------------->

![](img/0f16180ff8d436d385957f4c6f7dec21.png)

Source: [https://www.sudipta-deb.in/2019/02/quick-reference-of-arrow-function-in.html](https://www.sudipta-deb.in/2019/02/quick-reference-of-arrow-function-in.html)

箭头函数是标准 JavaScript 函数表达式的简洁替代，但是当我第一次看到它的语法时，我不知道如何解释那一行代码在做什么……更不用说为了搜索它而调用它了！让我们来看看它们什么时候有用，尤其是在涉及到`this`的时候。

## 语法:

您可能熟悉 JavaScript 中的标准函数表达式:

```
// standard function expression:const sum = function(num1, num2) {
  return num1 + num2
}sum(2, 3)
// 5
```

[箭头](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions#Function_body)函数可以有“简洁体”或通常的“块体”。***简洁的语法提供隐式返回值**，节省时间和代码行:*

```
*// concise body:const sum = (num1, num2) => num1 + num2sum(2, 3)
// 5*
```

*我们的`=>`替换了标准函数表达式中的`function`、`{}`和`return`，将代码的总行数减少到一行，并且仍然返回相同的结果。**块语法要求显式返回，但仍然节省时间和代码行:***

```
*// blocky body:const multiply = (num1, num2) => { return num1 * num2; };multiply(2, 3)
// 6*
```

## *何时= >有用:*

*对于需要隐式返回值的单行函数，使用`=>`。当你想多次应用一个函数时，箭头函数及其隐式返回**特别有用。例如，使用箭头函数使用`map`转换一个项目数组是轻而易举的事情:***

```
*// transforming an array of elements:const names = objects.map(object => object.name); // often seen in fetch requests:function fetchDogs(){
  fetch("[https://dog.ceo/api/breeds/image/random](https://dog.ceo/api/breeds/image/random)")
  .then(response => response.json())
  .then(addToDom)
}*
```

## *“这”这个词的意思是…*

*当涉及到面向对象 JavaScript 中的`this`时，箭头函数变得更加强大。arrow 函数没有自己的`this`值，但是它被定义 arrow 函数的代码块隐式绑定，也称为词法范围。*

*在 ES6 和 arrow 函数之前，在函数范围之间携带`this`的值有一些变通方法，比如将值设置为等于一个变量，比如使用 `.bind`的`that = this`或[。](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_objects/Function/bind)*

*箭头函数携带`this`的值，该值由定义该函数的块定义。还应注意，您不能使用箭头功能使用`.bind`改变或强制`this`的值。箭头函数将总是覆盖任何以前绑定的值。*

*要了解 move 这些强大的小功能能做什么，请查看以下资源:*

*   *[关于箭头功能的 MDN 文档](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)*
*   *[ES6 箭头功能真的能解决“这个”吗？](https://derickbailey.com/2015/09/28/do-es6-arrow-functions-really-solve-this-in-javascript/)*
*   *[ES6 箭头功能实用指南](https://blog.bitsrc.io/a-practical-guide-to-es6-arrow-functions-c16975100cf5)*