# Jest 测试中提高类型安全性的 3 种方法

> 原文：<https://javascript.plainenglish.io/3-ways-to-improve-type-safety-in-jest-tests-42957a362668?source=collection_archive---------7----------------------->

![](img/4be8e59a3a76e91ef2c4d307e5fcadf9.png)

Photo by [marcos mayer](https://unsplash.com/@mmayyer?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在单元测试中忽略类型安全和使用类型断言或 ts-ignore 注释通常更容易。随着时间的推移，这可能会导致过时的测试，并降低他们在生产中捕捉 bug 的能力。下面是一些使用 Jest 作为测试框架来引入更好的类型安全的方法。

# 1.在强制转换之前将对象分配给分部类型

假设我们有一个对象类型，它有很多属性，比如复杂的 React 组件的配置选项或属性。

忽略这一点的简单方法是将测试值转换为所需的类型。

```
type Properties = {a: *string*, b: *number, //...*}const identity = (properties: Properties) => properties*// Error: Argument of type '{}' is not assignable to parameter of type 'Properties'.* identity({});// No errors with type assertion
expect(identity({} as Properties)).toEqual({})
expect(identity({a: 123} as Properties).toEqual({a: 123})
```

一个更好的方法是将测试对象分配给`Partial`版本，这样对于我们选择提供的属性，值仍然被类型检查。

```
*// Error: Type 'number' is not assignable to type 'string | undefined'.ts(2322)* **const testProperties: Partial<Properties> = {a: 123};**expect(identity(testProperties as Properties))
  .toEqual(testProperties);
```

# 2.向 **jest 提供一个类型参数。类的模拟泛型**

给定一个接受类型`User`并返回用户名的函数，我们可以指定`jest.fn()`来替换模拟类。

```
class User {
   public name: *string*;
}const getName = (user: User) => user.name
const randomObject = {
   name: 12345,
   doesNotExist: ''
}const userMock = jest.fn().mockReturnValue(randomObject)// No errors despite the wrong types
expect(getName(userMock)).toEqual('kaylie')
```

最后一行似乎会导致类型错误，因为用`randomObject`直接调用 getName 会导致类型错误。这是因为 Jest mock 类型默认为 *any* ，不提供相同的类型安全。

```
// node_modules/@types/jest/index.d.tsinterface Mock<**T = *any*, Y extends *any*[] = *any***> extends Function, MockInstance<T, Y> 
{
     new (...args: Y): T;
     (...args: Y): T;
}
```

但是当我们将`User`传递给泛型类型参数时，我们通过额外的属性检查获得了更好的类型安全性。

```
const typedUserMock: jest.Mock<User> = jest.fn()// Argument of type '{ name: number; doesNotExist: string; }' is not // assignable to parameter of type 'User'. Types of property 'name' // are incompatible.
typedUserMock.mockReturnValue(randomObject)**typedUserMock.mockReturnValue({
   name: 'kaylie'
})**// Type safe, and passes!
expect(getName(typedUserMock)).toEqual('kaylie')
```

# 3.将 ReturnType 与 jest 结合使用。模拟函数

这里还有一个简单的例子，一个名为`filter`的函数将另一个函数作为参数。

```
type Predicate = () => *boolean*const filter = (predicate: Predicate) => { predicate()}
```

现在我们想写一个单元测试，当 filter 被调用时断言谓词被调用。我们希望确保`mockPredicate`与原始类型定义具有相同的签名。

```
const mockPredicate = jest.fn()// Error: *Type 'number' is not assignable to type 'boolean'.ts(2322)*
filter(() => 1)// No errors
filter(mockPredicate.mockReturnValue(1)**)**
```

与前面的例子类似，我们需要向泛型`jest.Mock`参数传递一个参数，但是这次使用正确的返回类型。

```
type Predicate = () => *boolean***const typedMockPredicate: jest.Mock<ReturnType<Predicate>> = 
jest.fn();**// Error: Argument of type '1' is not assignable to parameter of type 'boolean'.ts(2345)
filter(typedMockPredicate.mockReturnValue(1))// OK!
filter(typedMockPredicate.mockReturnValue(true))expect(typedMockPredicate).toHaveBeenCalled()
```

尽管示例使用了简单的 Jest 模拟，但是您也应该能够将它应用于 Jest 间谍和模块模拟。如果你正在寻找一种功能更全面的方法，请查看 [ts-auto-mock](https://typescript-tdd.github.io/ts-auto-mock/) 。感谢阅读！

## **简明英语笔记**

你知道我们推出了一个 YouTube 频道吗？我们制作的每个视频都旨在教给你一些新的东西。点击此处 查看我们，并确保订阅该频道😎