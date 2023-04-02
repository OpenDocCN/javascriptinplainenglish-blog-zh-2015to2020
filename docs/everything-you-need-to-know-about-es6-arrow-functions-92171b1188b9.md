# 关于 JavaScript 箭头函数您需要知道的一切

> 原文：<https://javascript.plainenglish.io/everything-you-need-to-know-about-es6-arrow-functions-92171b1188b9?source=collection_archive---------4----------------------->

## 使用 ES6 箭头函数编写清晰简洁的代码

![](img/427d1a029ab7432588b1ff25e9934a9b.png)

Photo by [Luca Bravo](https://unsplash.com/@lucabravo?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

ES6 为编写函数添加了一种新的语法，这种语法在当今的 JavaScript 世界中被大量使用。

在 ES6 之前，有两种主要的函数声明方式。

**1。函数声明语法:**

```
function add(a, b) {
 return a + b;
}
```

**2。函数表达式语法:**

```
const add = function(a, b) {
 return a + b;
};
```

**普通函数和箭头函数的第一个区别是编写函数的语法。**

使用 arrow 函数语法，我们可以将上面的 add 函数写成

```
const add = (a, b) => {
 return a + b;
};
```

除了箭头之外，你可能看不出有什么不同，但是如果我们在函数体中只有一行代码，我们可以将上面的箭头函数简化为

```
const add = (a, b) => a + b;
```

这里我们隐式地返回 a + b 的结果，所以如果只有一个语句，就不需要 return 关键字。
所以使用箭头函数会使你的代码更短。

> *如果我们想从 arrow 函数返回 object 怎么办？怎么才能写成单语句呢？*

当我们在花括号内添加一个语句时，它被认为是函数体的开始，如下所示:

```
const add = (a, b) => {
 return a + b;
};
```

所以要从 arrow 函数返回一个对象，我们需要用圆括号括起来。

```
const getObject = () => { 
  return { 
    name: 'Dan', 
    age: 30 
  };
};console.log(getObject()); // { name: 'Dan', age: 30 }
```

上述代码等效于以下代码:

```
const getObject = () => ({ name: 'Dan', age: 30 });console.log(getObject()); // { name: 'Dan', age: 30 }
```

如果您是 React 开发人员，那么这个语法在更改状态值时非常有用。
点击这里查看我之前关于它的文章

如果函数只有一个参数，那么我们也可以跳过括号

```
const add = (a) => a + 5;console.log(add(10)); // 15
```

与相同

```
const add = a => a + 5;console.log(add(10)); // 15
```

但是如果函数没有参数，加括号就很有必要了。

```
const add = () => 20 + 50;console.log(add()); // 70
```

**normal 函数和 arrow 函数的另一个区别是 arguments 对象。传递给函数的所有值都可以使用 arguments 对象来访问。**

```
function add() {
  let sum = 0;
  for(let i = 0; i < arguments.length; i++) {
    sum += arguments[i];
  }
  console.log(sum);
}

add(1,2,3,4,5); // 15
```

但是 arguments 对象在 arrow 函数中不可用。

```
const add = () => {
  let sum = 0;
  for(let i = 0; i < arguments.length; i++) {
    sum += arguments[i];
  }
  console.log(sum);
};

add(1,2,3,4,5); // Error: arguments is not defined
```

所以如果你正在使用 arguments 对象来访问传递给函数的值，你需要使用编写函数的旧语法来代替 arrow 函数。

**普通函数和箭头函数的主要区别在于*这个*关键字。**

这个上下文不再被绑定在箭头函数中。Arrow 函数使用函数所在的上下文定义为**这个**上下文。

看看下面的代码:

```
const display = () => {
  console.log(this);
}

display(); // it will print window object
```

如果你在浏览器中执行这段代码，你会看到**这个**指向窗口对象。

看看下面的代码:

```
const countries = {
   names: ["US", "INDIA", "JAPAN"],
   display: function() {
     console.log(this);
   }
};

countries.display(); // it will print countries object
```

在这种情况下，**这个**内部显示函数将引用`countries`对象本身，而不是`window`对象。

你能预测下面代码中这个的值吗？

```
const countries = {
   names: ["US", "INDIA", "JAPAN"],
   display: function() {
     setTimeout(function() {
       console.log(this);
     }, 1000);
   }
};

countries.display();
```

它将打印`window`对象，而不是`countries`对象。

我们可以用三种方法来解决这个问题。

1.  通过将**的上下文绑定到`setTimeout`的**回调函数

```
const countries = {
 names: ["US", "INDIA", "JAPAN"],
 display: function() {
  setTimeout(function() {
   console.log(this);
  }.bind(this), 1000);
 }
};countries.display();// it will print countries object
```

2.通过将显示函数的上下文分配给一个局部变量，并在`setTimeout`函数中使用它

```
const countries = {
 names: ["US", "INDIA", "JAPAN"],
 display: function() {
  const self = this;
  setTimeout(function() {
   console.log(self);
  }, 1000);
 }
};countries.display();
```

3.第三种也是最优选的方法是将`setTimeout`回调函数转换成箭头函数

```
const countries = {
 names: ["US", "INDIA", "JAPAN"],
 display: function() {
  setTimeout(() => {
   console.log(this);
  }, 1000);
 }
};countries.display();
```

因为 arrow 函数没有自己的上下文，所以它从定义函数的位置**获取上下文，即 countries 对象。**

所以现在，我们可以很容易地使用 arrow 函数打印出`setTimeout`函数中的所有国家，而不是使用 **self** 或 **bind** 方法

```
const countries = {
 names: ["US", "INDIA", "JAPAN"],
 display: function() {
  setTimeout(() => {
   this.names.forEach(country => console.log(country));
  }, 1000);
 }
};countries.display();
```

看看下面的代码:

```
// html 
<input id="btn" type="button" value="Click Me" class="btn" />// javascript
document.getElementById("btn").addEventListener('click', function() {
  console.log('first', this);
});
```

这里，我们为事件处理程序提供了一个普通的函数，因此**这个**将指向我们单击的按钮。

```
// html 
<input id="btn" type="button" value="Click Me" class="btn" />// javascript
document.getElementById("btn").addEventListener('click', () => {
  console.log('second',this);
});
```

这里我们为事件处理程序提供了箭头函数，所以在这种情况下，**这个**将不会指向按钮，而是指向窗口对象，它是定义函数的上下文。

所以要记住的一点是，如果你想保留**这个**上下文，你需要使用箭头函数，否则使用普通函数。

看看下面的代码:

```
const greeting = function(message) {
    return function(name) {
      console.log(message + " " + name);
    }
};

const hello = greeting('Hello');
console.log(hello('Adam')); // Hello Adam

const hi = greeting('Hi');
console.log(hi('Danny')); // Hi Danny
console.log(hi('Mike')); // Hi Mike
```

这里我们有一个问候函数，它返回一个函数。

我们可以使用箭头函数来简化它，如下所示:

```
const greeting = message => name => {
      console.log(message + " " + name);
};

const hello = greeting('Hello');
console.log(hello('Adam')); // Hello Adam

const hi = greeting('Hi');
console.log(hi('Danny')); // Hi Danny
console.log(hi('Mike')); // Hi Mike
```

因此

```
const greeting = function(message) {
    return function(name) {
      console.log(message + " " + name);
    }
};
```

相当于

```
const greeting = message => name => {
  console.log(message + " " + name);
};
```

此外，我们可以进一步简化为一行，如下所示:

```
const greeting = message => name => console.log(message + " " + name);
```

要理解这为什么有用，请查看我以前的文章[这里](https://medium.com/javascript-in-plain-english/super-short-syntax-of-writing-validation-functions-using-arrow-functions-in-es6-44c5a2f83e6)

## **结论**

正常功能和箭头功能
1 有 3 个主要区别。编写函数
2 的语法。Arguments 对象在箭头函数
3 中不可用。关于这个如何工作的区别

今天到此为止。希望你今天学到了新东西。

**别忘了直接在你的收件箱** [**这里**](https://yogeshchavan.dev) **订阅我的每周简讯，里面有惊人的技巧、窍门和文章。**