# JavaScript:object . define property

> 原文：<https://javascript.plainenglish.io/javascript-object-defineproperty-411c5fff06bc?source=collection_archive---------4----------------------->

![](img/d0a105e4a37f90187dedeb70fd060578.png)

Photo by [davisco](https://unsplash.com/@codytdavis?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/object?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

JavaScript 中的对象是属性的集合，属性是键*和值*的关联。创建一个对象非常简单:

```
const person = {
  name: 'Max',
  age: 23,
};
```

要在现有对象中定义新属性，您只需为其赋值:

```
person.gender = 'Male';
```

但是还有另外一种定义属性的方法，不太为人所知:`Object.defineProperty`。它需要三个参数:对象、属性名和描述符:

```
Object.defineProperty(*object*, *propertyName*, *descriptor*);
```

描述符，顾名思义，是描述属性的对象，我们将在下面的例子中看到。这种语法通常比较长，感觉不太自然，但是它可以做经典赋值不能做的事情。

# 可写、可枚举和可配置

我们分配给对象`person`的`gender`属性是

1.  可写的
2.  可列举的
3.  可配置的

第一点，可写，意味着您可以更改属性的值:

```
person.gender = 'Female';
console.log(person.gender); // Female
```

第二点，enumerable，意味着属性出现在对象属性的枚举中，例如在一个`for ... in ...`循环中:

```
for (const key in person) {
  console.log(key); // name age gender
}
```

最后，可配置的属性意味着您可以在以后删除该属性，或者更改它的可写和可枚举参数:

```
delete person.gender;
console.log(person.gender); // undefined
```

对我们来说，属性以这种方式运行是很自然的，但它们并不一定如此。默认情况下，用`Object.defineProperty`定义的属性是不可写、不可枚举、不可配置的。

```
Object.defineProperty(person, 'gender', {
  value: 'Male',
});// Non writable
person.gender = 'Female';
console.log(person.gender); // 'Male'// Non enumerable
for (const key in person) {
  console.log(key); // name age
}// Non configurable
delete person.gender;
console.log(person.gender); // 'Male'
```

因此，如果你的目标是实现这样的事情，使用`Object.defineProperty`可能是一个很好的选择`Proxy`(如果你从未听说过 JavaScript 代理，那么[这种方式](https://medium.com/javascript-in-plain-english/javascript-proxies-b41abcdd2bda))。

默认情况下，以这种方式定义的属性可能是不可写、不可枚举、不可配置的，但是您当然可以在描述符中更改它:

```
Object.defineProperty(person, 'gender', {
  value: 'Male',
  writable: true,
  enumerable: true,
  configurable: true,
});// Writable
person.gender = 'Female';
console.log(person.gender); // 'Female'// Enumerable
for (const key in person) {
  console.log(key); // name age gender
}// Configurable
delete person.gender;
console.log(person.gender); // undefined
```

# 存取描述符

在前面的例子中，我们使用*数据*定义了属性。我们使用描述符给它赋值，并声明它是可写的、可枚举的和可配置的。因此我们的描述符是一个*数据描述符*。还有另一种使用`Object.defineProperty` : *访问器描述符*定义属性的方法。

这些描述符有一个`get`和一个`set`方法，在其中你可以分别定义如何读写属性。

```
let value = 'Male';
Object.defineProperty(person, 'gender', {
  get: () => value,
  set: (newValue) => value = newValue,
});console.log(person.gender); // Male
person.gender = 'Female'; 
console.log(person.gender); // Female
```

默认情况下，这些方法返回`undefined`。通过不定义`set`方法，您将使您的属性不可写。

```
Object.defineProperty(person, 'gender', {
  get: () => 'Male',
});console.log(person.gender); // Male
person.gender = 'Female'; 
console.log(person.gender); // Male
```

同样，如果你想要做的只是使一个属性不可写，那么这可能是一个简短的替代方法。

值得注意的是，你可以使用一个*数据描述符* **或者**一个*访问器描述符*，但是你不能混合使用两者。尝试这样做将导致抛出错误。如果你试图改变一个不可配置的属性的配置，但是试图写一个不可写的属性，也会抛出一个错误。它只是不改变价值。

`Object.defineProperty`在大多数常见的 JavaScript 情况下是多余的。但它是一个很好的、本地的、可读的和易于实现的解决方案，在您不希望您的属性是可写的、可删除的或可枚举的情况下监视您的属性是如何被访问的。

## **简明英语团队的笔记**

你知道我们有四种出版物吗？给他们一个 follow 来表达爱意:[**JavaScript in Plain English**](https://medium.com/javascript-in-plain-english)[**AI in Plain English**](https://medium.com/ai-in-plain-english)[**UX in Plain English**](https://medium.com/ux-in-plain-english)[**Python in Plain English**](https://medium.com/python-in-plain-english)**—谢谢，继续学习！我们还推出了一个 YouTube，希望你能通过 [**订阅我们的简明英语频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) 来支持我们**

**一如既往,“简明英语”希望帮助推广好的内容。如果您有一篇文章想要提交给我们的任何出版物，请发送电子邮件至[**submissions @ plain English . io**](mailto:submissions@plainenglish.io)**，并附上您的媒体用户名和您感兴趣的内容，我们将会回复您！****