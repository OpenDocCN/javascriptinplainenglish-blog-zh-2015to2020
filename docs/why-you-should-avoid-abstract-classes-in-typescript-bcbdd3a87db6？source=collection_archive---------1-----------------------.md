# 为什么应该避免在 TypeScript 中使用抽象类

> 原文：<https://javascript.plainenglish.io/why-you-should-avoid-abstract-classes-in-typescript-bcbdd3a87db6?source=collection_archive---------1----------------------->

![](img/8cbb3f7675604b20d7fd5e87b0e35865.png)

Photo by [Steve Johnson](https://unsplash.com/@steve_j?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

TypeScript 中的抽象类是由关键字`abstract`定义的。它应该由其他类派生，而不是直接实例化。

下面的例子有 BaseLogger，它是一个抽象类，强制派生类实现它的`abstract`方法。

```
abstract class BaseLogger {
    abstract log(msg: string): void
}**// Error! Non-abstract class 'DebugLogger' does not implement inherited abstract member 'log' from class 'BaseLogger'.**class DebugLogger extends BaseLogger {
    **// log method must be implemented**
}
```

抽象类类似于具有方法的接口，但一个不同之处是它也可以包含共享实现。注意，这些方法的方法名前面没有`abstract`关键字。

```
abstract class BaseLogger {
    abstract log(msg: string): void public formatMessage(msg: string): string {
       return msg.toLowerCase();
    }
}class DebugLogger extends BaseLogger {

    public log(msg: string): void {
       const formattedMessage = this.formatMessage(msg); console.log(msg);
    }
}
```

理论上，这是一种很好的共享代码的方式。然而在实践中，我发现代码共享的好处经常被高估，特别是当抽象类的实现数量随着时间增长时。然后开始花更多的时间去理解抽象，而不仅仅是复制。当有真正可共享的代码时，**更喜欢组合而不是继承**。出于这个原因，我尽量避免抽象类，而更喜欢实用程序和接口，它们更符合单一责任原则。

```
**// Use interfaces to enforce implementation** export interface Logger {
   log(msg: string): void;
}**// Use utilities to share code** export const formatMessage = (msg: string) => msg.toLowerCase()// *in DebugLogger.ts*
class DebugLogger implements Logger {

   public log(msg): void {
       const formattedMessage = formatMessage(msg); console.log(msg);
   }
}
```

虽然上面的代码实现了相同的目标，但是提取的实用程序具有可测试性的额外好处。记住，你不能实例化抽象类，因此它们是不可独立测试的。虽然你可能认为抽象类是扩展类的实现细节，但这仍然意味着单元测试覆盖的代码量更少，比集成测试更便宜。

```
// Error! Cannot create an instance of an abstract class.
const baseLogger = new BaseLogger();
```

**最后，即使使用 VSCode IntelliSense，跟踪实现也需要更长的时间**。使用类而不是接口的一个好处是你的定义和实现都在一个地方，使用“转到定义”功能时只需快速按键。相比之下，如果您试图调试一个依赖于派生类实现的抽象类方法，它通常会让您搜索多个实现，在文件之间跳转。

```
abstract class BaseLogger {
    abstract log(msg: string): void public logWithInfo(msg: string): string {
       // Where is the implementation of this.log?
       return this.log(`${msg} + 'some extra info'); 
    }
}
```

总之，可读性、可测试性和开发人员工效学都是我尽可能避免抽象类的原因。