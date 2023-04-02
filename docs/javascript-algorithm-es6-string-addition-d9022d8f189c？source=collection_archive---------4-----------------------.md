# JavaScript 算法:ES6 字符串添加

> 原文：<https://javascript.plainenglish.io/javascript-algorithm-es6-string-addition-d9022d8f189c?source=collection_archive---------4----------------------->

## 我们将不用任何 JavaScript 内置方法和`+`符号来连接两个字符串。

![](img/55274d72106ada5548a39bda121d3399.png)

对于今天的算法，我们将编写一个名为`joinStrings`的函数，它接受两个字符串`string1`和`string2`作为输入。

该函数的目标是将两个字符串连接在一起，但是有一个问题。我们将连接两个字符串，不使用任何 JavaScript 内置方法，也不使用`+`符号。

ES6 引入了一种用字符串插值连接字符串的新方法。您可以使用模板文本将变量插入或插入字符串。模板文字的一个示例如下:

```
let myName = "Mud";
console.log(`My dog's name is ${myName}.`);
// outputs: My dog's name is Mud.
```

这种连接一个或多个字符串的新方法更容易读写。既然我们知道了如何在不使用内置方法的情况下连接两个字符串，我们可以写出我们的函数:

```
function joinStrings(string1, string2){
   return `${string1} ${string2}`;
}
```

如果您想了解更多关于字符串连接的知识，您可以查看我的一篇 JavaScript 基础文章，这篇文章涵盖了这方面的内容:

[](https://medium.com/@endubueze00/javascript-basics-string-concatenation-with-variables-and-interpolation-deba239debbe) [## JavaScript 基础:变量和插值的字符串连接

### 在 JavaScript 中，我们可以将字符串赋给一个变量，并使用串联将该变量组合成另一个字符串。

medium.com](https://medium.com/@endubueze00/javascript-basics-string-concatenation-with-variables-and-interpolation-deba239debbe) 

如果你觉得这个算法有帮助，可以看看我最近的其他 JavaScript 算法解决方案:

[](https://medium.com/javascript-in-plain-english/javascript-algorithm-first-reverse-85243cccdb3d) [## JavaScript 算法:先反向

### 我们要写一个函数，输出一个反过来写的字符串。

medium.com](https://medium.com/javascript-in-plain-english/javascript-algorithm-first-reverse-85243cccdb3d) [](https://medium.com/javascript-in-plain-english/javascript-algorithm-funny-string-adabc8eac4d7) [## JavaScript 算法:有趣的字符串

### 对于今天的算法，我们要写一个叫做 funnyString 的函数，它接受一个输入:一个字符串，s。

medium.com](https://medium.com/javascript-in-plain-english/javascript-algorithm-funny-string-adabc8eac4d7) [](https://levelup.gitconnected.com/javascript-algorithm-utopian-tree-b38100fff575) [## JavaScript 算法:乌托邦树

### 对于今天的算法，我们要写一个名为 utopianTree 的函数，它接受一个输入:一个整数 n。

levelup.gitconnected.com](https://levelup.gitconnected.com/javascript-algorithm-utopian-tree-b38100fff575)