# 19 个常见的打字稿面试问题

> 原文：<https://javascript.plainenglish.io/top-19-frequently-asked-typescript-interview-questions-dac4ff30c017?source=collection_archive---------1----------------------->

## 关于 TypeScript 的常见面试问题

![](img/8998a80484629124970d2a241326e86c.png)

Photo by [Sebastian Herrmann](https://unsplash.com/@officestock?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 介绍

如今，TypeScript 在网上无处不在，所以如果你是一名前沿开发人员，甚至是一名全栈开发人员，了解 TypeScript 的关键概念是很重要的。下面的一些问题可能也适用于其他面向对象语言，如 c#或 Java。

以下问题适用于所有级别。面试时，我通常对所有不同的级别进行相同的测试，因为这可以给你一个基线，让你知道你认为开发人员是什么水平。

# 打印面试问题

*   **使用 TypeScript 有什么好处？**

下面的一个或多个是你应该寻找的。

*   静态打字
*   新功能+浏览器兼容性
*   智能感知
*   键入注释
*   可读性
*   快速重构

*   **`**tsconfig.json**`**文件有什么用？****

**`tsconfig.json`文件指定了编译项目所需的根文件和编译器选项。**

*   ****如何在 TypeScript 中声明变量？****

**您在示例中寻找下面的。**

***例如:***

```
let name = "Dave"; // TypeScript will automaically create this as a string;let name: string = "Dave" // Using ":"
```

*   ****TypeScript 中哪个关键字用于继承？****

**`extends`是你应该寻找的关键词。**

***例子:***

```
class dog **extends** IAnimal
```

*   **在 TypeScript 中你能继承一个类吗？**

**是的，你可以使用关键字`extends`**

***例如:***

```
class Animal {
   numberOfLegs = 0;
}class dog **extends** Animal
```

*   **你能从不止一个类中继承吗？**

**不，你不能用打字稿。**

*   **有什么能帮到你的吗？**

**`mixins`创建分部类，我们可以将它们组合成一个包含分部类中所有方法和属性的类。**

*   ****TypeScript 中的 super 关键字是什么？****

**`Super`是一个 TypeScript 关键字，开发人员可以在表达式中使用，用于基类构造函数和基类属性引用。**

***例如:***

```
class Dog extends Animal {
  constructor(name) {
   ** super();**
    this.name = name;
    super.doSomething();
  }
}
```

*   ****什么是 Typescript 中的命名空间？****

**名称空间只是一种在包装器中对相关类或接口进行逻辑分组的方式。**

***例如:***

```
namespace Transport { 
   export class Car {  }  
   export class Bike {  }  
}
```

*   **Typescript 类中属性/方法的默认可见性是什么？**

**`public`是默认值**

*   **TypeScript 支持的所有其他访问修饰符是什么？**

**`public` -该类的所有成员、其子类以及该类的实例都可以访问。**

**该类及其子类的所有成员都可以访问它们。但是类的实例不能访问。**

**`private` -只有该类的成员可以访问它们。**

*   ****如何在 TypeScript 中定义可选属性？****

**你要找的是`?`还是`| undefined`**

***举例:***

```
// Example 1
interface ISomeInterface {
  foo?: string;
}// Example 2
interface ISomeInterface {
   foo: string | undefined;
}
```

*   ****TypeScript 中的装饰者是什么？****

**装饰器只是修改类、属性、方法或方法参数的函数。**

*   ****如何声明一个装饰者？****

**语法是“@”符号后跟一个函数。**

***例如:***

```
@readonly
class foo {
    name?: string;
}
```

*   ****什么是 TypeScript 中的函数重载？****

**函数重载是用不同的实现创建多个同名函数的能力。**

***例如:***

```
class Account {
    Id: number;

    search(Id: number);
    search(name:string);
    search(value: any) {
    if (value && typeof value == "number") {
        //Do something
    }
    if (value && typeof value == "string") {
        //Do Something
    }
   }
}
```

*   ****你能在 TypeScript 中访问静态方法吗？****

**是的，你可以！**

***例子***

```
class Foo {

    static Hello() { 
    }
} import { Foo } from "path/Foo";
Foo.Hello();
```

*   ****构造函数在 TypeScript 中有什么用？****

**构造函数负责初始化类的属性/变量。**

***例子***

```
class Foo {
   name = "";
   id?: number; constructor(id?: number, name: string) {
        this.id = id;
        this.name = name;
    }
}
```

*   ****TypeScript 中的** `**as**` **关键字是做什么用的？****

**`as`是 TypeScript 中类型断言的附加语法。**

***例如:***

```
let user = formData **as** User;
```

*   ****什么是 TypeScript 中的泛型？****

**使开发人员能够创建可以在多种类型而不是单一类型上工作的组件。这允许开发人员消费这些组件并使用他们自己的类型**

**可以使用`<T>`定义通用类/接口**

***例如:***

```
function someType**<T>**(anything: T): T {     
    return anything; 
}let myOutputString = someType<string>("myString");  // type of myOutputString variable will be 'string'let myOutputNumber = someType<number>(12); // type of myOutputNumber variable will be 'number'
```

# ****结论****

**我认为上面的问题给出了一个全面的打字稿面试，并允许开发人员展示他们的技能。有些问题非常具体地针对 TypeScript，而其他问题则侧重于 OOP 原则。**

**我认为光是提问并不是面试的最佳方式。与问题相关的其他想法:**

*   **编码挑战，像 Fizz Buzz 测试可能不是这个，因为我觉得每个人都听说过这个！**
*   **事先给他们一个任务来编码，然后在面试中向你解释他们做了什么**
*   **解释他们作为团队成员或个人参与的项目**

**[](https://medium.com/javascript-in-plain-english/24-quick-fire-javascript-interview-questions-a71f78d03f08) [## 24 个快速 JavaScript 面试问题

### JavaScript 快速面试问题

medium.com](https://medium.com/javascript-in-plain-english/24-quick-fire-javascript-interview-questions-a71f78d03f08) 

# **用简单英语写的便条**

你知道我们推出了一个 YouTube 频道吗？我们制作的每个视频都旨在教给你一些新的东西。点击 点击 [**查看我们，并确保订阅该频道😎**](https://www.youtube.com/channel/UCtipWUghju290NWcn8jhyAw)**