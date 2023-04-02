# JavaScript 技巧——好用的构造

> 原文：<https://javascript.plainenglish.io/javascript-tips-good-constructs-to-use-896937f9f5c9?source=collection_archive---------10----------------------->

![](img/429d635ab72492e11c94046e33332ad1.png)

Photo by [Blake Wisz](https://unsplash.com/@blakewisz?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 是一种非常灵活的语言。JavaScript 中有很多东西我们可能不应该写。不好的旧结构与更好的新结构共存。

在本文中，我们将看看 JavaScript 中一些我们应该经常使用的好的部分。

# 不要使用 var，使用 let 或 const

`var`是声明变量的过时关键字。相反，我们应该使用`let`和`const`。`let`和`const`都是块范围的，所以只要它们在一个块中，我们就可以不用担心冲突范围。

例如，我们可以编写以下内容:

```
let x = 1;
const y = 2;
```

而不是:

```
var x = 1;
var y = 2;
```

`const`还有一个好处就是我们不能给它重新赋值。然而，它并没有使对象成为不可变的。

# 用`=== or Object.is` 代替`==`

`==`节省了一个打字的字符，但是和`==`比较的规则比较复杂，很难记住。它还会产生许多意想不到的结果。所以，我们不应该用`==`。

例如，以下两者都返回`true`:

```
[10]  == 10
'10' == 10
```

我们可能不希望这样。因此，我们应该改为编写以下内容:

```
[10] === 10
'10' === 10
```

然后它们都返回`false`，因为两个操作数的数据类型和值必须匹配，以便`===`认为它们相同。但是，`NaN === NaN`返回`false`。

为了检查`NaN`，我们应该使用`isNaN`函数，该函数试图将其参数转换为一个数字并进行检查。因此，`isNaN(‘NaN’)` 和`isNaN(NaN)`都返回`true`。

还有`Number.isNaN`函数，它在检查时不做任何转换。因此，`Number.isNaN(‘NaN’)`返回`false`，`Number.isNaN(NaN)`返回`true`。

还有`Object.is`方法，它做的事情和`===`一样，只是`NaN`和它自己是一样的。

我们可以如下使用它:

```
Object.is('foo', 'bar')
```

论点是我们想要比较的两个东西。

# undefined，null，0，false，NaN，' '(空字符串)都是 false。

我们必须记住这些，因为当我们试图用`!!`或`Boolean`将它们转换成布尔时，它们都返回`false`。

# 使用分号来结束行

JavaScript 是宽容的，因为它不需要分号来结束行。它只会在换行符后或者任何它认为可以结束一行的地方插入分号。

然而，这会产生意想不到的结果。例如:

```
const foo = () => {
  return 
  {
    first: 'john'
  };
}
```

会返回`undefined`因为分号是加在`return`后面的，所以它下面的代码是永远不会运行的。

因此，我们应该根据需要插入分号，以明确一行的结束位置。

# 对对象构造函数使用类语法

在 JavaScript 中，类与对象构造函数相同。然而，语法更严格，所以更难出错，特别是当我们需要继承的时候。例如，我们可以编写下面的类:

```
class Person {
  constructor(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
  }
}
```

这与以下内容相同:

```
function Person(firstName, lastName){
  this.firstName =  firstName;
  this.lastName = lastName;        
}
```

只是`class`语法是更传统的语法，特别是对于那些使用其他面向对象语言的人来说。

如果我们有方法，那就更有意义了。例如，对于类语法，我们写:

```
class Person {
  constructor(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
  } fullName() {
    return `${this.firstName } ${this.lastName }`;
  }
}
```

上面的`Person`类有`fullName`方法。另一方面，使用旧的构造函数语法，我们编写:

```
function Person(firstName, lastName) {
  this.firstName = firstName;
  this.lastName = lastName;
}Person.prototype.fullName = function() {
  return `${this.firstName } ${this.lastName }`;
}
```

这对许多开发人员来说没有多大意义，尤其是对非 JavaScript 开发人员。

类语法使事情更加清晰。类语法对于继承更有意义。用旧语法，我们写:

```
function Person(firstName, lastName) {
  this.firstName = firstName;
  this.lastName = lastName;
}Person.prototype.fullName = function() {
  return `${this.firstName } ${this.lastName }`;
}function Student(studentId) {
  Person.call(this, studentId);
}
Student.prototype = Object.create(Person.prototype);
Student.prototype.constructor = Person;
```

当我们写上面的代码时，如果我们遗漏了什么，也不会有错误。

但是，对于类语法，我们写:

```
class Person {
  constructor(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
  }
  fullName() {
    return `${this.firstName } ${this.lastName }`;
  }
}class Student extends Person {
  constructor(firstName, lastName, studentId) {
    super(firstName, lastName);
    this.studentId = studentId;
  }
}
```

如果我们没有遵循语法，我们也会得到错误，比如当我们有`extends`时忘记调用`super`。

# 结论

我们应该使用一些重要的东西来编写 JavaScript 代码，包括用`===`代替`==`，用类语法代替构造函数和原型，用`let`和`const`代替`var`来声明变量和常量，并记住 falsy 值

## **用简单英语写的 JavaScript 笔记**

我们已经推出了三种新的出版物！请关注我们的新出版物:[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**——谢谢，继续学习！**

**我们也一直有兴趣帮助推广高质量的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的 Medium 用户名，我们会将您添加为作者。另外，请让我们知道您想加入哪个/哪些出版物。****