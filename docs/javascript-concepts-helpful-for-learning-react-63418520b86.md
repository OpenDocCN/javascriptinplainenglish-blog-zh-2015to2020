# 有助于学习 React 的 JavaScript 概念

> 原文：<https://javascript.plainenglish.io/javascript-concepts-helpful-for-learning-react-63418520b86?source=collection_archive---------6----------------------->

![](img/ada7c2b70f6d2c4ba67992e17312796b.png)

通过这篇文章，我想让你熟悉在 React 中广泛使用的 JS 概念。我已经有将近一年没有接触 JavaScript 了，然后就加入了 React 潮流。这就是为什么，我确实遇到了一些问题。因此，如果你打算学习 React，这里有一些你可能希望修改的概念。随意跳过你已经熟悉的部分。

*   **类**
*   **解构与展开操作者**
*   **箭头功能**
*   **模板字符串**
*   **声明变量**

**类:**

如果你有 OOPS 的背景，你可能会熟悉这个概念。类是用户定义的数据类型，用于存储相似的特征和行为。看看这个例子:

```
class Person {
  constructor(age, name) {
       this.name = name;
      this.age = age;
  } showDetails() {
    console.log(“Person Details : Age = “ + this.age + “ and name                                                      is: “ + this.name);
    }
}
```

这里，我们创建了一个“Person”类，它有两个特征，“name”和“age”，以及一个行为“showDetails”。我们现在可以使用以下内容创建一个“人”:

```
var person = new Person(20,“John”);
```

我们也可以继承类，这意味着使用其他类来扩展类的功能。假设我们需要创建一个“学生”类，一个学生总是有一个名字和一个年龄，所以我们可以在创建“学生”类的同时扩展“父”类。看一下语法:

```
class Student extends Person {
   constructor(id,age,name) {
      super(age,name);
 [this.id](http://this.id) = id;
   } showStudentDetails() {
      console.log(“Student has Id: ”+[this.id](http://this.id));
      super.showDetails();
   }
}
```

然后我们创建一个学生，如下所示:

```
var student  = new Student(1,20,”John”);
```

一些术语:
**构造函数**:这是一个创建对象时调用的函数。通常这是你初始化你的对象属性的地方。
**超级**:用于访问上级属性。在“学生”类中使用 super，其实是指向“人”类。

**解构与展开操作:**

这些概念在 React 中被大量使用，也是我最纠结的地方。假设你有一个像这样的物体:

```
var user = {
  name : “Steve”,
  age : 15,
  email : “abc@xyz.com”
}And you only need limited number of properties from this object. The traditional way to do it was :let name = user.name;
let age = user.age;
```

然而，现在我们可以用更好的方法来做。呈现**解构。**

```
let { name, age} = user;
This will take out only name and age from user object and assign the corresponding values to “name” and “age” variables.
```

比方说你要复制一个对象，不用手动一个一个的复制属性，我们可以使用 spread 运算符。

```
let copiedUser = …user.
```

这个奇怪的 3 点“…”叫做**传播算子**。顾名思义，它是用来传播的对象。在 React 上下文中，它主要用于组合对象或附加到对象。

```
To combine multiple objects :
let combinedObject = {…user,…category,…cart};This “combinedObject” will have all the properties present in “user”, “category” and “cart”.To append to an object(needs to be an array) :
let newArray = […previousArray, newValue];
```

这就是 Spread 运算符的神奇之处。

**箭头函数:** 这些是用来快速且以更干净的方式编写函数的。对于 React 来说，这不是必须要学的东西，但这肯定会提高你的工作效率。

```
function getParameter() {
   return param;
}getParameter = () => {
   return param; 
}Both of the above 2 functions have similar functionality.
Also, with arrow functions with parameter:getParameter = (param) => {
 return param;
}If there is only a return statement in arrow function, we can skip the curly braces:
getParameter = (param) =>  param;
```

与普通函数相比，Arrow 函数最大的优点之一是在其中使用了“this”关键字。
**常规函数中“this”关键字的值取决于函数是如何被调用的。arrow 函数中的“this”值取决于函数的定义位置。**

```
const obj = {
 name: "john",
 regularFunction :function() {
 console.log("regularFunction : " +this.name);   
 //John will be printed here
},arrowFn : () => {
     console.log("arrowFn : "+this.name);   
     //name is undefined when called because here "this" represents          context where its defined and that object doesn’t have name property     defined
 }
}obj.regularFunction();
obj.arrowFn();
```

**模板字符串:**

如果你看到我上面的代码，我是这样打印我的用户信息的:

```
console.log(“Person Details : Age = ”+this.age+” and name is: ”+this.name);
```

我们现在有了更好的方法来达到同样的效果。同样的语句可以写成

```
console.log(`Person Details : Age = ${this.age} and name is: ${this.name}`);
```

注意这里我们使用`(反斜杠)来打印。此外，${ }表示表达式，所以无论我们在${}中写什么，都将被求值。
例如:

```
 console.log(`2 + 5 is ${2+5}`); ==> 2 + 5 is 7
```

**声明变量:**

这个概念相对容易理解。有 3 种方式声明变量:
**let** ， **var** 和 **const** 。

**var :
用 var 关键字声明的变量被“提升”到块的顶部，这意味着它们甚至在声明之前就可以在它们的封闭范围内被访问。**

**let :** 用“let”声明的变量是块作用域，即只能在声明它的块中访问。

**const :** 用于标记一个变量常量，即以后不能更改。然而，正确的做法是，引用是不变的，不能改变。我们仍然可以改变变量的内部属性。

```
{ var a = 5;
 let b = 6; 
 const c = 7; 
 b = 8; 
 // c = 9         --> Not possible because c is constant reference}console.log(a);
//console.log(b);   -->  Not able to access "let" outside block
//console.log(c);   -->  Not able to access "const" outside block
```

**结论:**

在 React 中，你会经常用到这些概念。像创建组件、析构和扩展操作符的类，同时处理反应状态等等。当然，这篇文章会帮助你快速反应。
快乐编码..