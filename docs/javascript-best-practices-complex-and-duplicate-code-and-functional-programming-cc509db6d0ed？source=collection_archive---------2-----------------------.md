# JavaScript 最佳实践——复杂重复的代码和函数式编程

> 原文：<https://javascript.plainenglish.io/javascript-best-practices-complex-and-duplicate-code-and-functional-programming-cc509db6d0ed?source=collection_archive---------2----------------------->

![](img/05a89dcd6deecad5d101efe53c5af4da.png)

CFPhoto by [Andriyko Podilnyk](https://unsplash.com/@yirage?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

为了使代码易于阅读和维护，我们应该遵循一些最佳实践。

在这篇文章中，我们将看看我们应该遵循的一些最佳实践，以使每个人的生活更轻松。

# 删除重复代码

重复代码是错误的。

如果我们必须改变它们，我们必须在多个地方改变它们。

很容易错过他们。

因此，我们应该清除它们。

例如，不写:

```
function showEmployeeList(employees) {
  employees.forEach(employee => {
    const salaries = employees.salaries();
    const tasks = employees.tasks();
    const data = {
      salaries,
      tasks
    }; display(data);
  });
}function showManagerList(managers) {
  managers.forEach(manager => {
    const salaries = manager.salaries();
    const tasks = manager.tasks();
    const data = {
      salaries,
      tasks
    }; display(data);
  });
}
```

我们写道:

```
function showWorkersList(workers) {
  workers.forEach(worker => {
    const salaries = worker.salaries();
    const tasks = worker.tasks();

    const data = {
      salaries,
      tasks
    };

    switch (worker.type) {
      case "manager":
        render(data);
      case "employee":
        render(data);
    }
  });
}
```

# 使用 Object.assign 设置默认对象

将一个非空对象设置为`Object.assign`的第一个参数将返回一个对象，该对象在第一个参数中的所有属性与其他参数的属性合并。

例如，我们可以写:

```
const obj = Object.assign({
    title: "Foo"
  },
  config
);
```

`obj`是一个带有`title`的对象，其后带有`config`中的属性。

# 不要使用标志作为函数参数

我们不应该使用标志作为函数参数。

例如，我们可以写:

```
function getData(name, isManager) {
  if (isManager) {
    return findManagerData(name);
  } else {
    return findWorkerData(name);
  }
}
```

我们写道:

```
function getManagerData(name) {
  return findtManagerData(name);
}function getWorkerData(name) {
  return findtWorkerData(name);
}
```

# 没有副作用

副作用是对函数之外的东西产生的影响

有些副作用是不可避免的，比如操纵文件和改变其他外部资源。

剩下的大部分都可以避免。

例如，我们不应该写:

```
let name = "foo bar";function splitName() {
  name = name.split(" ");
}
```

因为我们可以用函数返回结果。

我们可以改为写:

```
function splitName(name) {
  return name.split(" ");
}
```

# 如果对象是从函数参数传递来的，就不要在原地操作它

所有非原始值，包括对象和数组，都通过引用传递给函数。

因此，如果我们修改这些参数，那么我们将改变原始对象。

例如，如果我们有:

```
const addTask = (tasks, task) => {
  tasks.push(task);
};
```

然后`tasks`数组和传入的`task object` 将被修改。

因此，我们应该返回一个新的数组:

```
const addTask = (tasks, task) => {
  return [...tasks, tasks];
};
```

这是我们可能没有想到的更隐蔽的副作用。

# 不要写全局函数

永远不要修改内置原型。

相反，我们可以扩展它们。

扩展内置原型会产生意想不到的结果。

它们使测试更容易，并防止错误改变数据。

例如，不写:

```
Array.prototype.itemsInOtherArray = function(arr) {
  return this.filter(element => !arr.includes(element));
};
```

我们写道:

```
class DiffArray extends Array {
  itemsInOtherArray(arr) {
    return this.filter(element => !arr.includes(element));
  }
}
```

# 使用函数式编程

我们应该使用函数式编程原则，比如不产生副作用。

此外，我们使对象不可变。

高阶函数也是我们可以使用的。

一些数组方法是展示所有这些特征的完美例子。

不使用循环计算总和:

```
let totalPrice = 0;

for (const item of items) {
  totalPrice += item.price;
}
```

我们写道:

```
const totalPrice = items.reduce(
  (totalPrice, output) => totalPrice + item.price,
  0
);
```

JavaScript 数组使用了`reduce`方法将数组条目中的总价相加。

# 封装条件

如果我们在条件中有很长的布尔表达式，我们可以把它们放在一个函数中。

例如，不写:

```
if (item.price > 100 && isLarge(item) && isSmart(item)) {
  // ...
}
```

我们可以写:

```
const isBigAndSmart = (item) => {
  return item.price > 100 && isLarge(item) && isSmart(item)
}if (isBigAndSmart(item)) {
  // ...
}
```

# 没有否定条件句

我们的代码中不应该有负条件。

它们很难读懂。

例如，不要写:

```
if (!isPresent(node)) {
  // ...
}
```

我们写道:

```
if (isPresent(node)) {
  // ...
}
```

![](img/beaede76deffdecdfbb15bf70f0bd9fc.png)

Photo by [Maud Slaats](https://unsplash.com/@maudslaats?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们应该利用 JavaScript 中的函数式编程特性。

此外，我们应该删除重复的代码，并将复杂的代码移到它们自己的功能中。

# 简单英语的 JavaScript

喜欢这篇文章吗？如果是这样，通过 [**订阅解码获得更多类似内容，我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **！**