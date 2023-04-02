# JavaScript:属性访问

> 原文：<https://javascript.plainenglish.io/javascript-property-accesses-5a1bebf351cf?source=collection_archive---------0----------------------->

## 如何在引擎盖下执行属性访问

![](img/098fabbf8823a1fd1ee7daf99937917a.png)

Photo by [Jonathon Young](https://unsplash.com/@jyoung?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/property-access?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

**注意:** *这篇文章有些超前，假设你对什么是* `*[[Prototype]]*` *及其工作原理有所了解。*

如果您使用过 JavaScript，那么您很可能遇到过对象和属性访问。这是它的样子

```
// object literal
const obj = {
    x: 52
};// property access
obj.x;
```

`obj.x`称为属性访问。这段代码实际上对`obj`执行了一个`[[Get]]`操作。让我们来看看它是如何工作的

# [[获取]]

内置操作首先查看有问题的对象是否具有属性。如果在对象上找到属性，则返回值。所以，如果我们考虑上面的例子，它将返回 52。

```
console.log(obj.x); //52
```

非常简单直接。然而，如果在对象中没有找到属性，`[[Get]]`算法遍历`[[Prototype]]`链，看看是否能在那里找到属性。无论如何，如果它没有找到被请求属性的值，它将返回值`undefined`。

**注** : *关于* `*[[Prototype]]*` *的讨论链条很长，如果我试图把它塞进这里，我会做得不公平。我将把它留到以后的文章中。现在，如果您不熟悉* `*[[Prototype]]*` *链，只需知道每个对象都有一个名为* `*[[Prototype]]*` *的属性，它是在创建对象时创建的，并被赋予对另一个对象的引用。每条* `*[[Prototype]]*` *链条的顶端都是* `*Object*` *基型的原型。我知道，如果你不熟悉的话会很困惑，但是我保证，我会在后面的帖子里讨论这个问题。*

让我们将它分解，并通过一些例子来看看它是如何工作的

1.  `[[Get]]`首先在对象中查找值。在这种情况下，会找到该属性，并将返回值

```
const obj = {
    x: 52
};obj.x; //52
```

2.如果它在对象中找不到属性，它会遍历`[[Prototype]]`链来寻找值。*在下面的示例中* `*Object.create()*` *创建了一个新的对象，该对象是`*y*``*[[Prototype]]*`*链接到传入的源对象。* `mObj`没有`x`属性，但`x`存在于[[原形]]链中。*

```
const obj = {
    x: 52
};const mObj = Object.create(obj);mObj.x; // 52;
```

3.如果它无论如何都找不到被请求的属性，它将返回值`undefined`。

```
const obj = {
    x: 52
};const mObj = Object.create(obj);mObj.x; // 52
mObj.y; // undefined
```

**注:** *在引擎盖下，对* `*[[Get]]*`的操作为 `*mObj.y*` *做了一点更多的“工作”，因为它不得不通过所有的* `*[[Prototype]]*` *环节去寻找* `*y*` *。*

# [[Put]]

不用说，如果有一个运算符来获取一个属性的值，那么显然就有一个运算符来设置一个属性的值。考虑以下示例

```
const obj = {
    x: 52
};obj.x = 32;obj.x; // 32
```

现在，当给一个属性赋值时，很容易会认为`[[Put]]`操作被执行，并且赋值，或者创建属性，然后赋值。没那么简单。有许多因素决定了`[[Put]]`运算符的行为，最明显的是属性是否存在于对象上。

让我们看看当找到属性时会发生什么:

1.  如果属性是 ***访问器描述符*** ，则调用 setter *(在本文后面的 getter&setter 一节中讨论)。*
2.  如果属性是一个数据描述符，且**可写描述符**设置为 false，则除非在`strict`模式下运行，否则将无声地失败。在`strict`模式下抛出错误。我在关于[对象不变性的帖子中谈到了**数据描述符**。](https://medium.com/javascript-in-plain-english/javascript-object-immutability-b6d7b1e0297)
3.  否则，将属性的值设置为正常值。

但是，当在对象上找不到属性时会发生什么呢？如果你阅读`[[Get]]`操作是如何工作的，你可能已经猜到了，但是它不仅仅是`[[Prototype]]`链接扫描，还有更多的规则。

有三(3)条规则来确定如果在所讨论的对象上找不到属性，则`[[Put]]`操作将如何工作:

1.  如果数据访问器属性在`[[Prototype]]`链**和**中的任何位置被发现，其可写描述符*(这里讨论的*[](https://medium.com/javascript-in-plain-english/javascript-object-immutability-b6d7b1e0297)**)如果**不是**设置为假，那么在所讨论的对象上创建 ***阴影属性*** ，并且在那里设置值。**
2.  *如果在`[[Prototype]]`链**和**中的任何位置找到数据访问器属性，并且它的可写描述符*(这里讨论的是*[](https://medium.com/javascript-in-plain-english/javascript-object-immutability-b6d7b1e0297)**)***被设置为 false，那么赋值将无声地失败，并且 ***影子属性*** 将不会**被创建。******
3.  ****如果属性在`[[Prototype]]`链中的位置更高，并且它是 ***设置器*** ，那么 set 函数总是被调用。一个 ***阴影属性*** 将不会在所讨论的对象上创建。当检索所述属性的值时，`[[Get]]`操作将遍历`[[Prototype]]`链并从中检索值。****

******注:** *一* ***影子属性*** *是一种既存在于物体本身又存在于更高的* `*[[Prototype]]*` *链中的属性。考虑一个名为* `*x*` *的属性，对象上的* `*x*` *属性会直接遮蔽可能出现在* `*[[Prototype]]*` *链中较高位置的任何* `*x*` *属性，因为查找 obj.x 会直接返回对象(链中最低的)的值，而不是查找链中较高的值。考虑以下示例*****

```
****const obj = {
  x: 52
};const mObj = Object.create(obj);// Here's how the following assignment works
// Since x isn't on mObj 
// [[Put]] Traverses the [[Prototype]] chain
// Finds the property  on obj
// Creates a shadow property on mObj
// Assigns the shadow property value of 32
mObj.x = 32;// Since x now exists on both mObj and obj 
// when we do a retrieval of x from mObj 
// the value 32 will be returned.
mObj.x; // 32, because x now exists on mObj
obj.x; // 52, nothing changes****
```

# ****吸气剂和沉降剂****

****到目前为止，您应该知道`[[Put]]`和`[[Get]]`操作完全控制了值的设置和检索以及操作的实际工作方式。****

****ES5 引入了一种方法，通过 Getters 和 Setters 在每个属性的基础上覆盖这些功能。您可以通过使用`Object.defineProperty()`方法来定义 Getters 和 Setters。如果你读过我关于[对象不变性](https://medium.com/javascript-in-plain-english/javascript-object-immutability-b6d7b1e0297)的帖子，你会对下面的例子很熟悉。****

```
****const obj = { //A getter for property x
    get x() {
        return this._x;
    },    //A setter for property y
    set y(value) {
        this._x = value;
    }
};Object.defineProperty(obj, 'y', {

    //A getter for y
    get: function() {
        return this._y;
    },     // A setter for y
    set: function(value) {
        this._y = value;
    }
});obj.x; // undefined 
obj.x = 52; 
obj.x; // 52obj.y; // undefined 
obj.y = 32; 
obj.y; // 32****
```

****在上面的例子中，我们通过以下方式在对象上创建属性****

*   ****使用对象文字****
*   ****使用显式定义****

****无论哪种方式，我们创建的属性实际上并不保存属性值，而是在访问时调用一个隐藏函数来获取或设置属性值。****

****本质上，getter 是调用隐藏函数来获取值的对象的属性，setter 是调用隐藏函数来设置值的对象的属性。****

****当你为一个属性定义了一个 getter 或者 setter 或者两者，它就变成了一个 ***访问器描述符*** ，你不需要为它们定义`value`或者`writable`描述符。****

# ****结论****

****这篇文章讲述了`[[Put]]`和`[[Get]]`操作如何设置或获取属性值。我们研究了在对象上找到属性时和在两个操作中都没有找到属性时应用的不同规则。我们还看了如何使用 Getters 和 Setters 以及它们是如何工作的。****

****所以现在你应该对所有这些是如何工作的有一个透彻的理解。让我知道你的财产访问的经验。你在使用它时遇到困难了吗？你有没有在你的代码中遇到过一些对你来说没有意义的关于属性访问的怪异之处？让我们在下面的评论中讨论吧。****

****干杯！****