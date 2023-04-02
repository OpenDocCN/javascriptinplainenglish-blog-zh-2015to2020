# 使用 JavaScript 的对象构造函数—第 1 部分

> 原文：<https://javascript.plainenglish.io/using-javascripts-object-constructor-part-1-b93e15b75fe2?source=collection_archive---------3----------------------->

![](img/f275e733ba1077865fdcf1018dfc8074.png)

Photo by [Andrew Pons](https://unsplash.com/@imandrewpons?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在 JavaScript 中，`Object`构造函数让我们用给定的值创建对象包装器。如果将`null`或`undefined`传递给对象构造函数，它将创建一个空对象。如果传入构造函数的值已经是一个对象，那么它将返回该对象。

`Object`构造函数有两个属性。它有一个始终为 1 的`length`属性，像所有其他对象一样，`Object`构造函数有一个原型来获取类型对象的所有附加属性。

`Object`构造函数有许多有用的方法，不用构造一个新对象就可以使用。该列表的第 1 部分如下:

## `Object.assign()`

方法制作一个对象的浅层拷贝。第一个参数是要将对象复制到的目标对象，第二个参数接受要复制的对象。请注意，如果源对象和目标对象具有相同的属性，源对象的属性值将覆盖目标对象中的属性值。例如，我们可以写:

```
const target = { a: 1, b: 2 };
const source = { b: 3, c: 4};const newObj = Object.assign(target, source);
console.log(newObj)
```

如果我们运行代码，我们将得到`{ a: 1, b: 3, c: 4}`。我们也可以复制数组。例如，我们可以写:

```
const targetArr = [1,2];
const sourceArr = [2,3];const newArr = Object.assign(targetArr, sourceArr);
console.log(newArr)
```

当我们运行上面的代码时，我们记录了`[2,3]`。对于数组，它将使用源数组覆盖整个目标数组。

## `Object.create()`

`Object.create()`方法创建一个新对象，将您传入的对象作为新对象的原型。例如，我们可以写:

```
const obj = { a: 1, b: 2, c: 3 };
const newObj = Object.create(obj);
console.log(newObj);
```

在上面的代码中，当我们记录`newObj`时，它没有自己的不是从`obj`继承的属性。这是因为我们只向构造函数传递了第一个参数，它是返回的对象的原型。如果我们想添加只在返回的对象中可用的属性，我们传入一个对象，将属性名作为键，将属性`writable`、`configurable`、`enumerable`和`value`作为属性名键的属性，这称为属性描述符。例如，我们可以写:

```
const obj = {
  a: 1,
  b: 2,
  c: 3
};const childObj = {
  a: {
    writable: true,
    configurable: true,
    value: 'hello'
  },
  d: {
    writable: true,
    configurable: true,
    value: 'hello'
  }
}
const newObj = Object.create(obj, childObj);
console.log(newObj);
```

在上面的代码中，`writable`意味着属性的值可以更改。`configurable`表示属性描述符可以被改变，并且属性是否可以从对象中删除。`enumerable`属性是指用`for...in`循环枚举属性时出现的属性，`value`是该属性的值。

如果我们想要创建一个属性不可更改的对象，那么我们将`writable`设置为`false`，如下面的代码所示:

```
const obj = {
  a: 1,
  b: 2,
  c: 3
};const childObj = {
  a: {
    writable: false,
    configurable: true,
    value: 'hello'
  },
  d: {
    writable: true,
    configurable: true,
    value: 'hello'
  }
}
const newObj = Object.create(obj, childObj);
newObj.a = 1;
newObj.d = 1;
console.log(newObj);
```

请注意，赋值运算符不起作用。如果我们启用了严格模式，那么就会抛出一个`TyepError`。我们已经在`console.log`中登录了`{a: “hello”, d: 1}`。这意味着设置为`false`的`writable`为属性`a`工作，设置为`true`的`writable`值为属性`d`工作。

如果我们将`null`传递给构造函数，我们会得到一个空对象:

```
const nullObj = Object.create(null)
console.log(nullObj)
```

我们将得到一个错误“对象原型可能只是一个对象或 null: undefined”是我们在`undefined`中传递的一个对象的原型，如下面的代码所示:

```
const undefinedObj = Object.create(undefined);
console.log(undefinedObj)
```

![](img/470b6ef349dfef19ca154b5a677f9327.png)

Photo by [Tierra Mallorca](https://unsplash.com/@tierramallorca?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## `Object.defineProperty()`

方法定义了一个对象的新属性。第一个参数是要向其添加属性的对象。第二个参数是您希望添加的属性的名称，作为字符串传递，最后一个参数是当我们试图向返回的对象添加属性时包含在`Object.create()`方法中的属性描述符。属性描述符应该将属性`writable`、`configurable`、`enumerable`和`value`作为属性名称键的属性。`writable`意味着属性的值可以改变。`configurable`表示属性描述符可以被改变，并且属性是否可以从对象中删除。`enumerable`属性是指用`for...in`循环枚举属性时出现的属性，`value`是该属性的值。例如，我们可以写:

```
let obj = {};Object.defineProperty(obj, 'a', {
  writable: false,
  configurable: true,
  value: 'hello'
})console.log(obj.a)
obj.a  = 1;
console.log(obj.a)
```

正如我们所见，属性描述符的行为与`Object.create()`中的行为相同。当`writable`为`false`时，对该属性的赋值无效。如果我们启用了严格模式，那么将抛出一个`TyepError`。

我们还可以为属性定义 getter 和 setter 函数，分别称为属性的`get`和`set`:

```
let obj = {};
let value;Object.defineProperty(obj, 'a', {
  get() {
    return value;
  },
  set(a) {
    value = a;
  }
});console.log(obj.a)
obj.a = 1;
console.log(obj.a)
```

正如我们所看到的，在上面的代码中，我们用`get`和`set`函数为对象`obj`定义了属性`a`，以分别获取属性的值和设置值。

如果我们在原型上定义了访问器属性，那么它是在原型上设置的，但是值属性是在当前对象上设置的。如果一个对象继承了不可写的属性，它在当前对象上仍然是不可写的。例如，如果我们有:

```
let ObjClass = function() {};
ObjClass.prototype.a = 1;Object.defineProperty(ObjClass.prototype, "b", {
  writable: false,
  value: 1
});const obj = new ObjClass();
ObjClass.prototype.a = 3
obj.a = 2
ObjClass.prototype.b = 3
obj.b = 2
console.log(obj);
```

那么分配给财产`b`是没有效果的。如果我们启用了严格模式，那么将抛出一个`TyepError`。

## `Object.defineProperties()`

方法让我们在一个对象上定义多个属性。该方法的第一个参数是要定义属性的对象，第二个对象包含作为键的属性名和作为值的相应属性描述符。属性描述符应该有属性`writable`、`configurable`、`enumerable`和`value`作为属性名称键的属性。`writable`表示属性值可以更改。`configurable`表示属性描述符可以被改变，并且属性是否可以从对象中删除。`enumerable`属性是指在用`for...in`循环枚举属性的过程中出现的属性，`value`是属性的值。例如，我们可以写:

```
let obj = {}
Object.defineProperties(obj, {
  a: {
    writable: true,
    configurable: true,
    value: 'hello'
  },
  d: {
    writable: false,
    configurable: true,
    value: 'hello'
  }
})
obj.a = 1;
obj.d = 1;
console.log(obj);
```

添加`a`和`d`属性，其中`a`的值可以更改，而`d`的值不能更改。由于属性`d`的描述符的`writable`属性被设置为`false`，所以`console.log`将显示`{a: 1, d: “hello”}`。我们还可以将`configurable`设置为`false`，以防止它被删除或其属性描述符被更改。例如，我们可以写:

```
let obj = {}
Object.defineProperties(obj, {
  a: {
    writable: true,
    configurable: true,
    value: 'hello'
  },
  d: {
    writable: false,
    configurable: false,
    value: 'hello'
  }
})
obj.a = 1;
delete obj.d;
console.log(obj);
```

如果我们运行上面的代码，那么我们可以看到`delete`操作符对`obj`的属性`d`没有影响。

## `Object.entries()`

`Object.entries()`方法返回一个对象的键值对数组，可由`for...in`循环枚举。如果使用`for...in`循环进行迭代，它们将按照相同的顺序返回。数组的每个条目将把关键字作为第一个元素，把相应关键字的值作为第二个元素。例如，我们可以在下面的代码中使用它来遍历对象的属性:

```
const obj = {
  a: 1,
  b: 2
};for (let [key, value] of Object.entries(obj)) {
  console.log(key, value);
}
```

当我们运行上面的代码时，我们得到了`a 1`和`b 2`。这些是这个对象的键值对。

## `Object.freeze()`

方法冻结一个对象。这意味着对象中的所有属性值都不能更改。此外，不能向其添加新属性，也不能更改冻结对象的现有属性描述符。物体被冻结在原地。该方法不返回新的冻结对象。相反，它返回冻结前的原始对象。冻结对象的原型也不能改变。例如，如果我们运行以下命令来冻结一个对象:

```
const obj = {
  a: 1
};Object.freeze(obj);obj.a = 2;console.log(obj.a);
```

我们得到`obj.a`仍然是 1。这是因为对象的属性值不能更改。如果我们启用了严格模式，将会产生一个`TypeError`。值得注意的是，作为对象的值仍然可以被修改。例如，如果我们有以下代码:

```
const obj = {
  a: 1,
  b: {
    c: 2
  }
};Object.freeze(obj);obj.b.c = 3;console.log(obj.b.c);
```

那么`obj.b.c`就是 3，因为`Object.freeze`不会冻结嵌套对象内部的属性，除非它们被显式冻结。所以如果我们有:

```
const obj = {
  a: 1,
  b: {
    c: 2
  }
};Object.freeze(obj);
Object.freeze(obj.b);obj.b.c = 3;console.log(obj.b.c);
```

那么`obj.b.c`就是 2，因为我们冻结了`obj.b`，所以我们不能修改`obj.b`的值。

`Object`构造函数有更多的方法来从一个数组构造对象，该数组具有属性的键和值，还有从对象、属性名、属性符号中获取属性描述符的方法，获取对象的键，防止属性被添加或删除或修改它们的属性描述符。