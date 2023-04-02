# 反映 API👌关于复杂理论的简单话语

> 原文：<https://javascript.plainenglish.io/reflect-api-simple-words-about-the-complex-theory-ec70c56cd558?source=collection_archive---------0----------------------->

[**反射 API**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Reflect) 揭露隐藏在普通 JavaScript 习惯用法背后的抽象操作。它的主要用途是提供合理的方法来转发代理陷阱上调用的操作。

**Reflect** 是一组用于处理对象的有用方法，其中一半重写了对象中已经存在的*。*

**这样做是为了改善语义和排序**，因为 Object 是基类，但是它也包含了很多不应该在其中的方法。同样，如果你用一个空的原型创建一个对象，那么你将会失去反射方法。

与大多数全局对象不同， **Reflect** 不是一个构造函数。不能将它与 new 运算符一起使用，也不能将 Reflect 对象作为函数调用。反射的所有属性和方法都是静态的。

e.g .我们可以创建一个对象，其中的字段**将在访问它们时自动创建:**

*   **Reflect.has()** 方法的工作原理类似于> ***中的<*** 运算符作为函数

```
// **Reflect.has()**const **MyObject** = {
  "**property_1**": 42
};console.log( **Reflect.has(MyObject**, "**property_1**"**)** );
// output: **true**console.log( **Reflect.has(MyObject**, "**property_2**"**)** );
// output: **false**
```

*   **Reflect.set()** 方法的工作原理类似于*设置一个对象的属性。*

```
// **Reflect.set()**const **SmthObject = {}**;
**Reflect.set( SmthObject**, '**property**', **42 )**;console.log( **SmthObject.property** );
// *output: 42*const **SmthArray** = ['duck', 'duck', 'duck'];
**Reflect.set( SmthArray**, **2**, '**goose**' );console.log( **SmthArray[2]** );
// *output: "goose"*
```

*   **Reflect.get()** 方法的工作原理类似于从一个对象获取一个属性(target[propertyKey]) **作为一个函数。**

```
const **SmthObject** = {
  x: 1,
  y: 2
};console.log( **Reflect.get(SmthObject, 'x')** );
// *output: 1*const **SmthArray** = ['zero', 'one'];console.log( **Reflect.get(SmthArray, 1)** );
```

# 那么为什么有必要使用反射❓呢

*   **反射 AP** I 更便于错误处理。例如，每个人都知道指令:

```
**Object.defineProperty (** obj, name, desc **)**
```

如果失败，将会抛出异常。

```
**try {**
   **Object.defineProperty(** obj, name, desc **)**;
   // property defined successfully
**} catch (e) {**
   // possible failure and might accidentally catch the wrong exception
**}**
```

Reflect 总是返回布尔值。

```
if ( **Reflect.defineProperty( SmtObj**, name, desc ) ) {
   // success
} else {
   // to do smth
}
```

*   **某些条件更短**😉

```
**Function.prototype.apply.call(** func, obj, args **)** **Reflect.apply.call(** func, obj, args **)**
```

*   **行为差异**💪

```
**Object.getPrototypeOf(** 1 **)**; // undefined**Reflect.getPrototypeOf(** 1 **)**; // TypeError - more logical 
```

*   **使用空原型**🥛

```
const **SmtObj** = Object.create(null);
**SmtObj**.bar = "value of eproperty";**SmtObj**.**hasOwnProperty** === ***undefined***; // true// SO YOU MUST TO WRITE THIS:**Object.prototype.hasOwnProperty.call(** **SmtObj**, 'bar' **)**; // true
```

> 我们没有**反射**的方法，例如 **hasOwnProperty()**

因此，我们可以用老🗝️的方法，参照*基类的原型，或者参照* ***反映 API* :**

```
**Reflect.ownKeys(** SmtObj **)**.**includes(**'bar'**)**
```

> 👏👏👏
> 
> 一般来说，Reflect API 看起来像是重构的结果。
> 
> 它包含以前在基类 Object、Function、**等**中保护的反射函数。
> 
> 如上所示，行为和错误处理发生了变化。

![](img/9d9154a4566ec3f663b7350d8b0fac28.png)

*   MDN Web 文档

[](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/API/proxy) [## 代理人

### 使用代理 API 代理 web 请求。有两种不同的方法可以做到这一点:

developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/API/proxy) [](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Reflect) [## 显示

### Reflect 是一个内置对象，为可拦截的 JavaScript 操作提供方法。方法与…相同

developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Reflect) 

*   Habr

[](https://habr.com/company/tuturu/blog/334546/) [## про反映 API

### Всем привет! Недавно услышал, как одни молодые фронтендеры пытались объяснить другим молодым фронтендерам, что такое…

habr.com](https://habr.com/company/tuturu/blog/334546/)