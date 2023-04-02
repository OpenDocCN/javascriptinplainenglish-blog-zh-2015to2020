# JavaScript 模式——模块和沙箱

> 原文：<https://javascript.plainenglish.io/javascript-patterns-modules-and-sandbox-76f8a3cf3b1c?source=collection_archive---------2----------------------->

![](img/6c767997b991dd69e4f8e200b9ddd10a.png)

Photo by [Cinzia Orsina](https://unsplash.com/@cinziafm?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

JavaScript 让我们可以做很多事情。它的语法有时过于宽容。

在本文中，我们将研究如何创建和使用对象，包括模块模式和沙箱模式。

# 创建构造函数或类的模块

我们可以拥有创建构造函数的模块。

例如，我们可以编写以下内容:

```
const Foo = (() => {
  class Foo {
    //...
  }
  return Foo;
})();
```

我们可以创建一个类并返回它。

同样，我们可以对构造函数做同样的事情。

我们可以创建一个构造函数并返回它:

```
const Foo = (() => {
  function Foo() {
    //...
  } Foo.prototype.fn = function() {
    //...
  }
  return Foo;
})();
```

这和创建一个类然后返回它没有什么不同。

然后我们可以在模块中使用构造函数，如下所示:

```
const foo = new Foo();
```

# 将全局变量导入模块

我们可以让我们的生活接受参数，这样我们就可以把全局变量传递给它。

例如，我们可以写:

```
const module = ((global) => {
  //...
})(this);
```

现在我们可以在安全的环境中使用全局变量了。

# 沙盒模式

沙箱模式解决了上面使用的名称空间模式的问题。

它解决了依赖单个全局变量作为应用程序的全局变量。

还有，我们有带点的名字，比其他名字长。

我们可以如下使用沙盒模式:

```
new Sandbox((box) => {  
  // ... 
});
```

我们将一个函数作为基本模式传递给构造函数。

我们可以传入一个字符串数组来传递模块名。

例如，我们可以写:

```
Sandbox(['foo', 'bar'], (box) => {
  // console.log(box);
});
```

`'foo'`和`'bar'`是模块名。

我们可以多次启动沙盒。因此，如果我们想要启动包含不同模块的，我们可以编写:

```
Sandbox(['foo'], (box) => {
  // console.log(box);
});
```

并且:

```
Sandbox(['foo', 'bar'], (box) => {
  // console.log(box);
});
```

为了实现沙盒模式，我们可以编写以下代码来包含我们的模块:

```
class Sandbox {
  constructor(modules, callback) {
    if (!(this instanceof Sandbox)) {
      return new Sandbox(modules, callback);
    }
    for (const name of Object.keys(Sandbox.modules)) {
      Sandbox.modules[name](this);
    }
    callback(this);
  }
}
Sandbox.modules = {}Sandbox.modules.foo = (box) => {
  box.foo = "bar";
};Sandbox.modules.bar = (box) => {
  box.getBar = () => {
    console.log('bar');
  };
};const sandbox = new Sandbox(['foo', 'bar'], (box) => {
 console.log(box)
})
```

在上面的代码中，通过用`this`回调模块来初始化沙箱。

然后可以用`box`参数访问我们的沙箱。

我们是这样做的:

```
for (const name of Object.keys(Sandbox.modules)) {
  Sandbox.modules[name](this);
}
```

此外，我们用`this`调用了构造函数中的`callback`参数，这样我们也可以在回调中访问我们的类实例。

我们可以添加更多的检查，看看每个模块和回调是否是函数。

例如，我们可以写:

```
class Sandbox {
  constructor(modules, callback) {
    if (!(this instanceof Sandbox)) {
      return new Sandbox(modules, callback);
    }
    for (const name of Object.keys(Sandbox.modules)) {
      const fn = Sandbox.modules[name];
      if (typeof fn === 'function') {
        Sandbox.modules[name](this);
      } if (typeof callback === 'function') {
        callback(this);
      }
    }
  }
}
Sandbox.modules = {}Sandbox.modules.foo = (box) => {
  box.foo = "bar";
};Sandbox.modules.bar = (box) => {
  box.getBar = () => {
    console.log('bar');
  };
};const sandbox = new Sandbox(['foo', 'bar'], (box) => {
  console.log(box)
})
```

现在我们检查所有东西都是函数。

我们还可以向类中添加额外的方法，如下所示:

```
class Sandbox {
  constructor(modules, callback) {
    if (!(this instanceof Sandbox)) {
      return new Sandbox(modules, callback);
    }
    for (const name of Object.keys(Sandbox.modules)) {
      const fn = Sandbox.modules[name];
      if (typeof fn === 'function') {
        Sandbox.modules[name](this);
      } if (typeof callback === 'function') {
        callback(this);
      }
    }
  } hello() {
    //...
    console.log('hello');
  }
}
Sandbox.modules = {}Sandbox.modules.foo = (box) => {
  box.foo = "bar";
};Sandbox.modules.bar = (box) => {
  box.getBar = () => {
    console.log('bar');
  };
};const sandbox = new Sandbox(['foo', 'bar'], (box) => {
  box.hello();
})
```

我们添加了`hello`方法，它将在沙箱的原型中。

然后我们可以通过`box`参数访问它来调用它。

![](img/a98c731c33fd8f8c1dff679b04742e64.png)

Photo by [pooya ramezani](https://unsplash.com/@pooya_ramezani?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以在 IIFEs 中返回构造函数或类。这样，我们可以在内部保存一些私有变量并使用它们。

为了使命名空间更加灵活，我们可以使用沙箱模块。

我们获取模块的名称，然后在我们的函数`this`中调用它们，这样它们就可以访问我们的构造函数和类实例。

此外，我们对传入的回调也做了同样的处理，以便回调可以访问我们的构造函数或类实例。

## **说白了**

通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **来表达爱意吧！**