# JavaScript 命名约定最佳实践

> 原文：<https://javascript.plainenglish.io/javascript-naming-convention-best-practices-b2065694b7d?source=collection_archive---------1----------------------->

## 通过示例介绍 JavaScript 命名约定

![](img/30ee2f65afc259015efa026f0d3a965c.png)

Photo by [Jeremy Bishop](https://unsplash.com/@jeremybishop?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## TL；速度三角形定位法(dead reckoning)

没有人强制执行这些命名约定规则，但是，它们在 JS 社区中被广泛接受为标准

## JavaScript 命名约定:变量

JavaScript 变量区分大小写。因此，具有小写和大写字符的 JavaScript 变量是不同的

**JavaScript 变量应该是自描述的。没有必要为变量添加附加文档的注释**

最常见的 JavaScript 变量是用一个带前导小写字符的变量名称**来声明的**

## JavaScript 命名约定:布尔型

像`is`、`are`、`has`这样的前缀可以帮助每一个 JavaScript 开发人员通过查看来区分一个布尔值和另一个变量

## JavaScript 命名约定:函数

JavaScript 函数也是在 **camelCase** 中编写的，最好的做法是通过给 ***函数名一个动词作为前缀*** 来告诉*函数在做什么*

这个动词作为前缀可以是任何东西(例如， *get，push，apply，calculate，compute，post )*

## JavaScript 命名约定:类

与其他 JavaScript 数据结构不同，JavaScript 类是用 **PascalCase** 声明的

## JavaScript 命名约定:组件

组件也广泛地用 **Pascal Case** 声明

当一个组件被使用时，它将自己与本地 HTML 和 web 组件区分开来，因为它的首字母总是大写的

## JavaScript 命名约定:方法

与 JavaScript 函数相同，JavaScript 类上的方法是用 camelCase 声明的

另外，添加一个**动词作为前缀**,使方法名更具自我描述性

## JavaScript 命名约定:私有

**下划线(_)** 为 JavaScript 中的私有变量。例如，类中的私有方法只能由该类内部使用，但应避免在该类的实例上使用

## JavaScript 命名约定:常量

JavaScript 中的常量——旨在成为不变的变量——用**大写字母(大写)**书写

## 结论

**变量**:驼峰

```
var firstName = "GP";
```

**布尔**:类似`is` `are` `has`的前缀

```
var isVisible = true;  
var areEqual = false;  
var hasEncryption = true;
```

**功能**:驼峰

```
function getName(firstName, lastName) {   
  return `${firstName} ${lastName}`; 
}
```

**类别**:帕斯卡案件

```
class SoftwareDeveloper {   
  constructor(firstName,lastName) {     
    this.firstName = firstName;     this.lastName = lastName;   
  } 
}
```

**组件**:帕斯卡塞

```
function UserProfile(user) {   
  return ( ... ) 
}
```

**方法**:山茶花

```
class SoftwareDeveloper {   
  getName() { return `` }; 
}
```

**Private** :下划线(_)

```
class SoftwareDeveloper {   
  constructor(firstName, lastName) {     
    this.name = _getName(firstName, lastName);   
}    
  _getName(firstName, lastName) {     
    return `${firstName ${lastName}`;   
  } 
}
```

**常量**:大写字母(大写)

```
var SECONDS = 60; 
var MINUTES = 60; 
var HOURS = 24;  
var DAY = SECONDS * MINUTES * HOURS;
```

> 干净的编码！

![](img/251b0306dc50e0e269bb03972a092150.png)

Photo by [Daiga Ellaby](https://unsplash.com/@daiga_ellaby?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## **用简单英语写的 JavaScript**

喜欢这篇文章吗？如果有，通过 [**订阅我们的 YouTube 频道**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw) **获取更多类似内容！**