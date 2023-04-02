# 我是如何提高打字技能的

> 原文：<https://javascript.plainenglish.io/how-i-improve-my-skills-in-typescript-79caa9e745a4?source=collection_archive---------11----------------------->

![](img/399bcd37b953abc77a9af7e6670445af.png)

我将分享一些帮助我提高打字技巧的技巧！

# 打字警卫

Typeguard 允许您在条件块中验证对象的类型。

```
interface Fish {
  swim: () => void
}

interface Bird {
  fly: () => void
}

function isFish(pet: Fish | Bird): pet is Fish {
  return (pet as Fish).swim !== undefined
}
```

由于这个条件，我们可以确定这个宠物是一个`Fish`。

**为什么以及在哪里使用这个？**

当您需要在其他类型中检查一个对象的类型时，这非常有用。在上面的例子中，typeguard `isFish`可以这样使用。

```
function toto(pet: Fish | Bird) {
  if (isFish(pet)) {
     pet.swim() // At this moment, TS know that pet is `Fish` and no a `Bird`
  }
}

function isFish(pet: Fish | Bird): pet is Fish {
  return (pet as Fish).swim !== undefined
}
```

# 映射类型

当我想定义一个对象的可能键时，我经常使用它。

```
type PossibleKeys = 'a' | 'b' | 'c'

type Toto = {
   // This is a mapped type
  [key in keyof PossibleKeys]: string
}

const toto: Toto = { ... } // Only key allowed are a, b or c !
```

# 键入`this`作为函数中的参数

一个小提示:您可以像这样在函数中键入`this` object

```
function toto(this: { a: string }, arg: number) {
  console.log(this.a, arg) // "toto",  55 
}

toto.bind({ a: 'toto' })(55) // Don't forget to bind `this`
```

# 推断类型

通过`generic type`，你可以使用一个条件来`infer`输入。`infer`是什么意思？推断类型是 TypeScript 查找对象的正确类型的能力。

```
type NonNull<T> = T extends (null | undefined) ? never : T

const toto: NonNull<undefined> = undefined // TS Error since object type (toto) is never, it's not possible to create this
const toto: NonNull<string> = 'tt' // No error since object type (toto) is string !
```

# 实用程序类型

TypeScript 允许我们使用实用类型，这是一个非常有用的功能！您可以在[*https://www . typescriptlang . org/docs/handbook/utility-types . html*](https://www.typescriptlang.org/docs/handbook/utility-types.html)获得完整列表

我将向您展示我使用的常见实用程序类型。

**部分**:

构造一个类型，该类型的所有属性都设置为可选。

```
interface Toto { a: string, b: string }
// Partial type equal to -> interface Partial<Toto> { a?: string, b?: string }

const partialToto: Partial<Toto> = { a: 'a' }
```

`Pick`用于从一个类型中提取一些键，以便创建一个新的类型。

```
interface Toto { a: string, b: string }
// Pick type equal to -> interface Pick<Toto, 'a'> { a: string }

const pickToto: Pick<Toto, 'a'> = { a: 'a' }
```

`Omit`用于从一个类型中删除一些键以便创建一个新的类型。

```
interface Toto { a: string, b: string }
// Pick type equal to -> interface Omit<Toto, 'a'> { a: string }

const omitToto: Omit<Toto, 'a'> = { b: 'b' }
```

有了三种实用程序类型，您可以创建一种新的非常智能的类型！对于其他开发人员来说理解起来非常有用。

**记录**:

你可以用类型化的键和类型来构造一个对象，并像这样创建有用的类型

```
type TotoKeys = 'a' | 'b' | 'c'
interface Toto { name: string, age: number }

const toto: Record<TotoKeys, Toto> = {
   a: { name: 'a', age: 55 },
   b: { name: 'b', age: 44 },
   c: { name: 'c', age: 33 },
}
```

我喜欢记录，因为你可以使用 enum 来键入键。

```
enum TotoEnum { 
  A = 'A',
  B = 'B',
  C = 'C'
}
interface Toto { name: string, age: number }

const toto: Record<TotoEnum, Toto> = {
   [TotoEnum.A]: { name: 'a', age: 55 },
   [TotoEnum.B]: { name: 'b', age: 44 },
   [TotoEnum.C]: { name: 'c', age: 33 },
}
```

我希望你通过阅读这篇文章来学习和提高你的技能！

如果您有其他建议或问题，请在下面评论。

希望你喜欢这篇读物！

🎁如果你在[的推特](https://twitter.com/code__oz)上关注我并给我打 MP，你可以免费得到我的新书**被低估的 javascript 技能，改变现状**😁

或者在这里得到它

🎁[我的简讯](https://www.getrevue.co/profile/code__oz)

☕️你能支持我的作品吗🙏

🏃‍♂️，你可以跟着我👇

🕊 [推特](https://twitter.com/code__oz)

👨‍💻 [Github](https://github.com/Code-Oz)

你可以标记🔖这篇文章！

*更多内容尽在*[***plain English . io***](http://plainenglish.io/)