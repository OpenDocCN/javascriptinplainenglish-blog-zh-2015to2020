# 编写简洁 JavaScript 的 15 条规则

> 原文：<https://javascript.plainenglish.io/15-rules-for-writing-clean-javascript-8e2b2b426515?source=collection_archive---------2----------------------->

## 来自《可维护的 JavaScript》一书

![](img/a52ae91a545cbd0a8dd3bc29025cea79.png)

Photo by [Sarah Dorweiler](https://unsplash.com/@sarahdorweiler?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/clean-javascript?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

所以你是 React 开发者或者 Node.js 开发者。你可以写出有用的代码。但是你能写出视觉上很漂亮并且别人能理解的代码吗？

今天我们将看到一些让你的 JavaScript 代码干净清晰的规则。

## 规则一。不要使用随机字符作为变量

不要用一些随机的字符来表示一个变量。

```
//BAD const x = 4;
```

正确命名变量，使其正确描述值。

```
//GOODconst numberOfChildren = 4;
```

## 规则二。使用 camelCase 变量名

不要使用 snake_case、PascalCase 或以动词开头的变量名。

```
// Bad: Begins with uppercase letter var UserName = "Faisal"; -----// Bad: Begins with verb var getUserName = "Faisal"; -----// Bad: Uses underscore var user_name = "faisal";
```

相反，使用代表名词的骆驼大小写变量名。

```
// Goodconst userName = "Faisal";
```

## 规则三。使用好的 camelCase 函数名

不要使用任何名词作为函数名，以避免与变量名混淆。

```
// Bad: Begins with uppercase letter 
function DoSomething() {  
    // code 
} ----// Bad: Begins with noun 
function car() {  
    // code 
} ----// Bad: Uses underscores 
function do_something() {  
    // code 
}
```

取而代之的是，名字以动词开头，并使用大小写。

```
//GOODfunction doSomething() {
    // code
}
```

## 规则 4。使用 PascalCase 进行构造函数命名

```
// Bad: Begins with lowercase letter 
function myObject() {  
    // code 
} ----// Bad: Uses underscores 
function My_Object() {  
    // code 
} ----// Bad: Begins with verb 
function getMyObject() {  
    // code 
}
```

此外，构造函数名应该以非动词开头，因为 new 是创建对象实例的动作。

```
// GOODfunction MyObject() {  
    // code 
}
```

## 规则五。全局常数

值不变的全局常量不应该像普通变量那样命名。

```
// BADconst numberOfChildren = 4;// BAD const number_of_children = 4;
```

它们应该都是大写字母，并用下划线分隔。

```
// GOODconst NUMBER_OF_CHILDREN = 4;
```

## 规则 6。变量赋值

不要将比较值赋给没有括号的变量。

```
// BAD const flag = i < count;
```

在表达式两边使用括号:

```
// GOODconst flag = (i < count);
```

## 规则 7。等式运算符的使用

不要用“==”或者”！= "来比较值。因为它们不会在比较前进行检查。

```
//BADif (a == b){
    //code
}
```

相反，请始终使用“===”或“！== "以避免类型强制错误。

```
//GOODif (a === b){
    //code
}
```

## 规则 8。三元运算符的用法

不要使用三元运算符作为 if 语句的替代:

```
//BADcondition ? doSomething() : doSomethingElse();
```

仅在某些条件下使用它们来赋值:

```
// GOODconst value = condition ? value1 : value2;
```

## 规则 9。简单语句

虽然 JavaScript 支持。不要在一行中写多条语句。

```
// BADa =b; count ++;
```

相反，多行多语句。并且总是在一行的末尾使用分号。

```
// GOODa = b;
count++;
```

## 规则 10。If 语句的使用

不要在 if 语句中省略括号，也不要将它们放在一行中。

```
// BAD: Improper spacing 
if(condition){  
    doSomething(); 
} ----// BAD: Missing braces 
if (condition)  
    doSomething(); ----// BAD: All on one line 
if (condition) { doSomething(); } ----// BAD: All on one line without braces 
if (condition) doSomething();
```

始终使用括号和适当的间距:

```
// GOODif (condition) {
    doSomething();
}
```

## 第 11 条。For 循环的用法

不要在 for 循环的初始化中声明变量。

```
// BAD: Variables declared during initialization for (let i=0, len=10; i < len; i++) {  
    // code 
}
```

在循环之前声明它们。

```
// GOODlet i = 0;for (i=0, len=10; i < len; i++) {  
    // code 
}
```

## 第 12 条。一致的缩进长度

坚持一直用 2 或 4。

```
// GOODif (condition) {
    doSomething();
}
```

## 第 13 条。线长度

任何一行都不应超过 80 个字符。如果超过这个数目，它们应该被分成一个新的行。

```
// BAD: Following line only indented four spaces doSomething(argument1, argument2, argument3, argument4,
    argument5); ----// BAD: Breaking before operator 
doSomething(argument1, argument2, argument3, argument4
        ,argument5);
```

第二行应该缩进 **8 个空格**而不是 4 个，并且不应该以分隔符开始。

```
// GOOD
doSomething(argument1, argument2, argument3, argument4,
        argument5);
```

## 第 14 条。原始文字

字符串不应该使用单引号。

```
// BADconst description = 'this is a description';
```

相反，他们应该总是使用双引号

```
// GOODconst description = "this is a description";
```

## 规则 15:使用“未定义”

切勿使用未定义的特殊值。

```
// BADif (variable === "undefined") {  
    // do something 
}
```

要查看变量是否已经定义，使用`typeof`操作符

```
// GOODif (typeof variable === "undefined") {  
    // do something 
}
```

因此，通过遵循这些规则，您可以使您的 JavaScript 项目更加整洁。

这些规则摘自“ **Nicholas C. Zakas** 写的《**可维护的 Javascript** 》一书。所以如果你不同意某些观点，我想那也没关系。谈到造型，没有单一的方式。但是这里的规则可以成为你的一个很好的起点。

今天到此为止。编码快乐！:D

**通过**[**LinkedIn**](https://www.linkedin.com/in/56faisal/)**或我的** [**个人网站**](https://www.mohammadfaisal.dev/) **与我取得联系。**