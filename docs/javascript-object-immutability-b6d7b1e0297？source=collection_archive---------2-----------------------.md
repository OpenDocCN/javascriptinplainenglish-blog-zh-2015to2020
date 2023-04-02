# JavaScript 对象不变性

> 原文：<https://javascript.plainenglish.io/javascript-object-immutability-b6d7b1e0297?source=collection_archive---------2----------------------->

## 了解所有这些是如何工作的

![](img/aad9e4b42aeaaeefb6146db110e2d39c.png)

在 JavaScript 中有三种方法可以实现对象的不变性。

*   object . prevent extension(obj)；
*   object . seal(obj)；
*   object . freeze(obj)；

为了理解 JavaScript 中对象不变性的工作原理，你必须首先理解任何对象的属性是如何用 ***描述的*** *。*

# 属性描述符

JavaScript 中的对象有属性，从 ES5 开始，所有属性都用 ***属性描述符*** 来描述。您可以通过以下方式获取任何对象中属性的描述符

```
Object.getOwnPropertyDescriptor(obj, 'property');
```

这返回的是一个包含属性值及其特征的对象，*如何描述*。这是你如何使用它

```
const obj = {
    x: 'some value of x'
}const descriptors = Object.getOwnPropertyDescriptor(obj, 'x');console.log(descriptors);/* Returns
Object { 
    value: 'some value of x', 
    writable: true, 
    enumerable: true, 
    configurable: true 
}*/
```

默认情况下，对象属性描述符(即可写、可配置和可枚举)都设置为真。我们将在这一部分详细讨论每一个问题。

要更改属性的描述符，可以使用`Object.defineProperty(obj, prop, descriptor)`方法。它需要的三个参数是

*   **obj:** 要定义属性的对象
*   **属性:**要定义或修改的属性名
*   **描述符:**被定义或修改的属性的描述符。

*有一点需要注意的是，当使用* `*defineProperty*` *方法给* ***对象添加*** *属性时，新属性的描述符默认为* ***假*** *。在上面的例子中，所有描述符都被默认设置为真的原因是因为我们没有使用* `*defineProperty*` *方法来定义‘x’。与上面的例子相比，考虑下面的例子。*

```
const obj1 = {};Object.defineProperty(obj1, 'prop1', {
  value: 42,
});const descriptors = Object.getOwnPropertyDescriptor(obj1, 'prop1');console.log(descriptors);/* Returns
Object { 
    value: 42, 
    writable: false, 
    enumerable: false, 
    configurable: false 
}*/
```

## 可写的

当可写描述符设置为 true *，*时，您可以为属性赋值和重新赋值。这里有一个例子

```
let myObject = {
    x: 'someValue'
};myObject.x = 'some other value';
myObject.x = 52;
```

一切按预期运行。然而，当**可写描述符**的值设置为 false 时，您将无法更改该值。

让我们看看这将如何工作。

```
let myObject = { x: 52 }; // writable is trueObject.defineProperty(myObject, 'x', {
    writable: false
});// The following will silently fail
myObject.x = 42;
```

当重新分配属性值时，除非您打开了`'use strict'` *(严格模式)*，否则重新分配会自动失败。如果打开了严格模式，JavaScript 将抛出一个错误，告诉您该属性是只读的。

```
Error: "x" is read-only
```

## 可配置的

当这个描述符设置为 true 时，您可以更改属性的描述符。这里有一个例子

```
let myObj = {
    x: 52
};// By default the configurable descriptor on 'x' is set to true so you can do the following i.e. you can configure it.Object.defineProperty(myObj, x, {
    enumerable: false
});
```

但是，当**可配置描述符**设置为 false 时，您将无法更改属性描述符*(除了* ***可写*** *和* ***值*** *，您将无法配置属性)。不仅如此，你也不能从对象中删除属性。当您希望防止代码的其他部分意外删除某个属性时，这尤其有用。这是它看起来的样子*

```
let myObj = { x: 32 };Object.defineProperty(myObj, 'x', {
    configurable: false
});// Other than the 'writable' and 'value' descriptors, you can't re-configure this property;// You also can't delete the property
delete myObj.x;
```

在将可配置描述符设置为 false 之后，如果您试图在严格模式下重新配置一个属性，它将抛出以下错误

```
'use strict';const object1 = { x: 2 };Object.defineProperty(object1, 'x', {
  configurable: false
});Object.defineProperty(object1, 'x', {
  enumerable: false
});// Error: can't redefine non-configurable property "x"
```

当你试图删除属性时，它会抛出

```
'use strict';const object1 = { x: 2 };Object.defineProperty(object1, 'x', {
  configurable: false
});delete object1.x;
console.log(object1.x);// Error: property "x" is non-configurable and can't be deleted
```

## 可列举的

当此描述符设置为 true 时，将在迭代对象键时读取该属性。这意味着密钥将出现在`Object.keys()`中的`for…in`循环中。这里有一个例子

```
const obj = {
    x: 52 //enumerable is true
};Object.defineProperty(obj, 'a’, {
    value: 1 // enumerable is false
});Object.defineProperty(obj, 'b’, {
    value: 1, 
    enumerable: true // self explanatory
});obj.c = 53; // When creating properties by setting them, the descriptors are defaulted to true.for (key in obj) {
    console.log(key) // x, b, c
}console.log(Object.keys(obj)); // [x, b, c]
```

您还可以通过对对象使用`propertyIsEnumerable` 方法来检查属性是否是可枚举的，如下所示

```
obj.propertyIsEnumerable('x'); // true
obj.propertyIsEnumerable('a'); // false
obj.propertyIsEnumerable('b'); // true
obj.propertyIsEnumerable('c'); // true
```

# 对象不变性

终于！既然我们知道了对象属性是如何工作的，以及它们是如何被描述的，那么理解对象不变性是如何工作的就非常容易了。实现不同级别的对象不变性的三种方法是

*   Object.preventExtensions()
*   Object.seal()
*   Object.freeze()

这些方法本质上利用属性描述符来使对象不可变。让我们一个一个来看

## Object.preventExtensions()

默认情况下，对象是可扩展的，这意味着可以向它们添加属性。要检查一个对象是否是可扩展的，你可以像这样使用`isExtensible` 方法

```
const someObject = {};
Object.isExtensible(someObject); // true
```

这可以通过使用像这样的`preventExtensions` 方法来改变

```
Object.preventExtensions(someObject);
Object.isExtensible(someObject); // false
```

现在，当您尝试向该对象添加属性时，将会抛出一个错误

```
Object.defineProperty(someObject, 'nProp', { value: 52 });
//  Error: can't define property "nProp": Object is not extensible
```

当您希望对象的用户能够使用它做任何事情，但不添加更多属性时，这非常有用。

**注:**这里要注意的一点是`Object.preventExtensions()`只阻止属性给对象它自己。您仍然可以向对象的`[[Prototype]]`添加属性。JavaScript 中的`[[Get]]`和`[[Put]]`算法如何工作是另一篇[文章](https://medium.com/@haseebkhan_90/javascript-property-accesses-5a1bebf351cf?source=friends_link&sk=12598e8824cb095f33ca04a202b3efbb)的主题。

## Object.seal()

该方法调用对象上的`Object.preventExtensions()`，并将所有属性的可配置描述符设置为 false。这确保了你的对象的用户不能添加更多的属性或者从对象中删除*(删除)*属性。这里有一个例子

```
const obj = {
  x: 42
};Object.seal(obj);obj.x = 52;
console.log(obj.x); // 52delete obj.x; // cannot delete when sealed
console.log(obj.x); // 52Object.defineProperty(obj, 'y', { value: 24 });
//  Error: can't define property "y": Object is not extensible
```

## Object.freeze()

这就像它听起来的那样。它通过使用 Object.preventExtensions、Object.seal 冻结对象，并使所有属性的可写描述符为 false。这是你所能拥有的最高形式的不变性。它防止向其中添加新属性，防止移除现有属性，防止更改现有属性的可枚举性、可配置性或可写性，并防止更改现有属性的值。此外，它还防止修改对象的`[[Prototype]]`。

```
'use strict';const obj = { 
    x: 52 
};Object.freeze(obj);obj.x = 32;
// Error: "x" is read-onlyobj.y = 52; 
// Error: can't define property "y": Object is not extensibleObject.defineProperty(obj, 'y', { value: 52 });
// Error: can't define property "y": Object is not extensibleObject.defineProperty(obj, 'x', { enumerable: false });
// Error: can't redefine non-configurable property "x"Object.defineProperty(obj, 'x', { writable: true});
// Error: can't redefine non-configurable property "x"
```

# 结论

一旦你知道不同的属性描述符是如何工作的，并随着不同的方法而改变，理解**对象不变性**就变得容易多了。现在，您应该已经了解了不同的可用属性描述符，如何修改它们，修改它们的副作用，以及如何在对象不变性中使用它们。现在，您还应该知道可以用来实现不同级别的对象不变性的三种方法，以及它们在幕后是如何工作的。

您以前是否修改过属性描述符？让对象不可变怎么样？请在下面的评论中告诉我你对这些东西的体验，以及你在使用它们时遇到的任何问题。

干杯！