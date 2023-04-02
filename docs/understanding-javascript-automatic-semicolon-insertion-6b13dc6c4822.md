# 理解 JavaScript 自动分号插入

> 原文：<https://javascript.plainenglish.io/understanding-javascript-automatic-semicolon-insertion-6b13dc6c4822?source=collection_archive---------7----------------------->

## 如果您决定避免使用分号，请小心

![](img/6b424bcba20ddd192d8c6cd0e1e1026f.png)

在 javascript 中，分号的使用是可选的，在我们省略分号的情况下，JavaScript 使用自动分号插入(ASI)并假定一个“；”在你的 JS 代码的某些地方，即使你没有在那里写。

JavaScript 这样做是因为如果我们省略了一个必需的“；”我们的程序会失败，但有时这个 javascript 行为会做一些不必要的事情。

操作很简单:当 JS 解析器发现一行需要一个“；”它被省略了，它插入了一个。ASI 只能在有换行符的情况下工作。分号不在一行的中间。

下面是一个例子，分号应该终止这些变量:

```
let counter = 100
let ciunter2 = 0
```

由我们插入:

```
let counter = 100;
let ciunter2 = 0;
```

在这种情况下，语法要求我们用“；”写“counter”在“做”之后的结尾..虽然”，但我们大多数人都忘记了这一点，这是 ASI 非常有用的一个例子:

```
let counter= 100;do {
   //do some stuff
  counter++;
}
while (counter)//-> ";" expected here
```

另一方面，语句块不需要末尾的分号，所以 ASI 不做任何事情。

```
let counter = 100;while (counter){//do some stuff}// ";" No required here.
```

但是，在有些情况下，这个特性会改变代码的预期行为，而抛出语法错误会更好。ASI 引起的最典型的问题是 return、break 和 continue 关键字:

```
function foo(value){
 if(value) return
 console.log(value);
}
foo(100); //undefined.function foo(value){
 if(value)
  return  console.log(value);
}
foo(100) //100
```

ASI 的常见问题是从函数中返回“undefined”或者创建全局变量，所以如果你使用这个特性，了解语法规则以及自动插入是如何运行的是很重要的。

## 结论

我不喜欢依赖 ASI，但这是我个人的偏好。如果 ASI 失败，并且您缺少分号，代码将失败，但是另一方面，分号是可以避免的干扰。

在我的例子中，我更喜欢写分号，因为你可以理解其他任何人的代码，因为它遵循相同的规则和他写代码时的意图，如果将来 ASI 的操作发生变化，我的代码仍然可以工作。

不要忘记，如果您不想使用这个特性，避免它的唯一方法是确保分号被输入到正确的位置。